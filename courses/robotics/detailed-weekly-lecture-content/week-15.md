# Week 15: The Autonomous Navigation Pipeline

## Learning Objectives

By the end of this lecture, you should be able to:

1. Describe how mapping, estimation, planning and control fit together in one navigation system.
2. Explain what Nav2 provides, and how the hand-built pipeline relates to it.
3. Assemble a minimal pipeline in simulation that drives a robot to a goal around obstacles it did not know about.
4. Show with an experiment which components matter, by removing them one at a time.
5. Describe the structure of the same system as a ROS 2 node.

## 1. Integrating the Pieces

There is almost no new theory this week. Every part of the system has been built by hand in an earlier lecture, and the task is to connect them, which turns out to be where most of the difficulty lies.

```
LiDAR/Camera --> Occupancy Grid (Wk 9) --> Path Planner (Wk 13/14, A*/RRT)
      |                                           |
      v                                           v
Encoders/IMU --> Kalman Filter (Wk 12) --> Current Pose Estimate --> PID Controller (Wk 10) --> /cmd_vel
```

Read the diagram as two flows. Sensing flows to the left of the picture: the scanner feeds the map, and the encoders and any correcting sensors feed the estimator, which produces the pose. Decisions flow to the right: the planner uses the map and the pose to produce a path, and the controller uses the path and the pose to produce velocity commands. The pose is the hub, since the map is built with it, the plan starts from it, and the controller steers by it. An error in the pose therefore spreads everywhere, which is why Week 12 mattered.

## 2. The ROS 2 Navigation Stack (Nav2): Overview

Nav2 is the production-grade ROS 2 package for navigation. It provides localisation, mapping, global and local path planning, control, recovery behaviours and a way to describe missions, all configurable, and tested on many robots. It follows the same design as the pipeline above, with global planner plugins that include grid-based A* variants, local controllers that follow the path while avoiding obstacles, and layered cost maps that inflate obstacles.

We treat Nav2 as a reference point: it is what the hand-built pipeline is a simplified version of. You are not required to use it for the capstone, and ambitious students may explore it as an extension. A good way to understand Nav2 after this course is to read its configuration file and match each entry to a part that you built: the robot radius is your inflation, the planner is your A*, the controller is your PID, and the AMCL localiser is a particle-filter cousin of your Kalman filter.

## 3. Assembling a Minimal Pipeline

The loop that we implement has five steps, repeated at every control cycle.

1. Sense. Read the scan from `/scan` and the odometry from `/odom`, plus any correction signal.
2. Estimate. Fuse the odometry with the correction using the Week 12 filter pattern, to get the current pose.
3. Plan. Update the occupancy grid with the new scan, inflate it, and, if there is no path or the path has become blocked, run A* from the current position to the goal.
4. Control. Take a waypoint a little way ahead on the path, compute the heading error to it, and turn it into a velocity command with proportional or PID control. Publish it on `/cmd_vel`.
5. Repeat. A simple replanning loop, rather than the full theory of dynamic replanning.

### 3.1 The simulated world

To run this on any computer, we simulate everything, with no ROS. The world is the 10 by 10 metre room of last week, with a dividing wall that has a gap. The robot starts at `(1, 1)` and must reach `(9, 9)`. The robot knows the wall, but there is a box that is not on its map, standing close to the straight route from the gap to the goal. It must discover the box with its scanner, and avoid it.

```python
import math
import heapq
import numpy as np
from scipy.ndimage import binary_dilation

RES = 0.1                                   # metres per grid cell
N = 100                                     # 100 x 100 cells = 10 m x 10 m
WALL = [(4.5, 0.0, 5.5, 6.0), (4.5, 7.5, 5.5, 10.0)]     # rectangles as (x0, y0, x1, y1), with a gap in between
HIDDEN_BOX = [(7.0, 7.2, 8.0, 8.6)]                       # not on the robot's initial map

def blocked(x, y, rects):
    if not (0 <= x <= 10 and 0 <= y <= 10):
        return True
    return any(x0 <= x <= x1 and y0 <= y <= y1 for x0, y0, x1, y1 in rects)

def rasterise(rects):
    return np.array([[1 if blocked((c + 0.5) * RES, (r + 0.5) * RES, rects) else 0
                      for c in range(N)] for r in range(N)])

def cell(x, y):
    return int(y // RES), int(x // RES)          # (row, col)

def centre(row, col):
    return (col + 0.5) * RES, (row + 0.5) * RES

def wrap(a):
    return (a + math.pi) % (2 * math.pi) - math.pi

print("cells covered by the known wall:", int(rasterise(WALL).sum()))
print("cells covered by the hidden box:", int(rasterise(HIDDEN_BOX).sum()))
```

### 3.2 The simulated sensor

A very simple LiDAR: we step along each ray in small increments until it meets an obstacle.

```python
def raycast(origin, angle, rects, max_range=4.0, step=0.05):
    d = 0.0
    while d < max_range:
        x = origin[0] + d * math.cos(angle)
        y = origin[1] + d * math.sin(angle)
        if blocked(x, y, rects):
            return d
        d += step
    return math.inf

print("range to the wall from (3, 3) looking along +x:", round(raycast((3.0, 3.0), 0.0, WALL), 2), "m (expected about 1.5)")
print("range to the wall from (3, 7) looking along +x (through the gap):", raycast((3.0, 7.0), 0.0, WALL, max_range=1.9))
```

### 3.3 Mapping and planning

The map starts with the wall only, and the scanner adds whatever it sees, using the robot's estimated pose, not the true one. That is an important detail: a real robot has no access to the true pose. The planner inflates the map by the robot's radius plus a margin, 0.3 metres here, and runs A* from Week 13, with eight-connected moves.

```python
def inflate(grid, radius_cells):
    r = radius_cells
    yy, xx = np.mgrid[-r:r + 1, -r:r + 1]
    return binary_dilation(grid == 1, structure=(xx ** 2 + yy ** 2) <= r * r).astype(int)

def a_star(grid, start, goal):
    rows, cols = grid.shape
    moves = [(-1, 0), (1, 0), (0, -1), (0, 1), (-1, -1), (-1, 1), (1, -1), (1, 1)]
    def h(a):
        dx, dy = abs(a[0] - goal[0]), abs(a[1] - goal[1])
        return (dx + dy) + (math.sqrt(2) - 2) * min(dx, dy)          # octile distance
    best = {start: 0.0}
    parent = {start: None}
    frontier = [(h(start), 0.0, start)]
    while frontier:
        f, g, cur = heapq.heappop(frontier)
        if g > best[cur]:
            continue
        if cur == goal:
            path = []
            while cur is not None:
                path.append(cur)
                cur = parent[cur]
            return path[::-1]
        for dr, dc in moves:
            nxt = (cur[0] + dr, cur[1] + dc)
            if not (0 <= nxt[0] < rows and 0 <= nxt[1] < cols) or grid[nxt]:
                continue
            if dr and dc and (grid[cur[0] + dr, cur[1]] or grid[cur[0], cur[1] + dc]):
                continue                                              # no corner cutting
            new_g = g + math.hypot(dr, dc)
            if new_g < best.get(nxt, float("inf")):
                best[nxt] = new_g
                parent[nxt] = cur
                heapq.heappush(frontier, (new_g + h(nxt), new_g, nxt))
    return None

known_map = rasterise(WALL)
safe = inflate(known_map, 3)
initial_path = a_star(safe, cell(1.0, 1.0), cell(9.0, 9.0))
print("initial plan on the known map:", len(initial_path), "cells, passes through the gap:",
      any(abs(centre(*c)[0] - 5.0) < 0.5 and 6.0 < centre(*c)[1] < 7.5 for c in initial_path))
box_cells = [tuple(c) for c in np.argwhere(rasterise(HIDDEN_BOX) == 1)]
print("initial plan crosses the hidden box:", any(c in set(box_cells) for c in initial_path))
```

The first plan goes through the gap, and on this map it also crosses the hidden box, since the robot has no way of knowing that the box is there. It will only find out when it sees it.

### 3.4 The estimator

We reuse the Week 12 filter, once for x, once for y and once for the heading. The prediction step takes the odometry increment, and every second a correction arrives: a noisy position fix, with a standard deviation of 0.3 metres, and a noisy compass reading, with a standard deviation of 0.05 radians. In a real system these might come from a beacon system and a magnetometer, or from matching scans to a map.

```python
class KalmanFilter1D:
    def __init__(self, initial_estimate, initial_uncertainty, process_var, measurement_var):
        self.x, self.p, self.q, self.r = initial_estimate, initial_uncertainty, process_var, measurement_var

    def predict(self, motion=0.0):
        self.x += motion
        self.p += self.q

    def update(self, measurement):
        k = self.p / (self.p + self.r)
        self.x += k * (measurement - self.x)
        self.p *= 1 - k
```

### 3.5 The whole loop

Now the main loop. The code is long, so read it in the order of the five steps in the comments. Notice what is real and what the robot does not know: the true pose moves according to the commands, with a small unmodelled turn, as if one wheel were slightly faster. The odometry knows only the commands, plus noise.

```python
def run(seed=0, use_estimator=True, max_time=150.0, dt=0.1):
    rng = np.random.default_rng(seed)
    world = WALL + HIDDEN_BOX                       # what really exists
    known = rasterise(WALL)                         # what the robot believes at the start
    goal = (9.0, 9.0)
    goal_cell = cell(*goal)

    true = [1.0, 1.0, 0.0]                          # x, y, heading: unknown to the robot
    odo = [1.0, 1.0, 0.0]                           # pure dead reckoning
    kx = KalmanFilter1D(1.0, 0.05, 0.002, 0.09)
    ky = KalmanFilter1D(1.0, 0.05, 0.002, 0.09)
    kth = KalmanFilter1D(0.0, 0.01, 0.001, 0.0025)

    path, wp, replans, min_clearance, t = None, 0, 0, float("inf"), 0.0
    outcome = "ran out of time"
    while t < max_time:
        # 1 and 2. SENSE and ESTIMATE: the pose the robot believes in
        if use_estimator:
            ex, ey, eth = kx.x, ky.x, kth.x
        else:
            ex, ey, eth = odo
        # scan from the TRUE pose, but place the hits using the ESTIMATED pose
        for a in np.deg2rad(np.arange(-60, 61, 4)):
            r = raycast((true[0], true[1]), true[2] + a, world)
            if r < 4.0:
                row, col = cell(ex + r * math.cos(eth + a), ey + r * math.sin(eth + a))
                if 0 <= row < N and 0 <= col < N:
                    known[row, col] = 1

        # 3. PLAN: replan if there is no path, or if the next part of the path is blocked
        planning = inflate(known, 3)
        here = cell(ex, ey)
        planning[here] = 0
        planning[goal_cell] = 0
        if path is None or any(planning[c] for c in path[wp:wp + 25]):
            path, wp = a_star(planning, here, goal_cell), 0
            replans += 1
            for radius in (2, 1):                    # relax the safety margin if the map has closed the way
                if path is not None:
                    break
                relaxed = inflate(known, radius)
                relaxed[here] = 0
                relaxed[goal_cell] = 0
                path = a_star(relaxed, here, goal_cell)
            if path is None:
                outcome = "no path found"
                break

        # 4. CONTROL: steer to a waypoint three cells ahead
        while wp < len(path) - 1 and math.hypot(centre(*path[wp])[0] - ex, centre(*path[wp])[1] - ey) < 0.3:
            wp += 1
        tx, ty = centre(*path[min(wp + 3, len(path) - 1)])
        error = wrap(math.atan2(ty - ey, tx - ex) - eth)
        omega = float(np.clip(2.0 * error, -1.5, 1.5))
        v = 0.5 * max(0.0, math.cos(error))          # slow down when pointing the wrong way
        if math.hypot(goal[0] - ex, goal[1] - ey) < 0.25:
            outcome = "believes it has arrived"
            break

        # the real robot: the commanded motion plus an unmodelled turn (one wheel is slightly faster)
        true[2] += (omega + 0.03 * v / 0.3) * dt
        true[0] += v * math.cos(true[2]) * dt
        true[1] += v * math.sin(true[2]) * dt
        if blocked(true[0], true[1], world):
            outcome = "COLLISION"
            break
        for x0, y0, x1, y1 in world:
            gap_x = max(x0 - true[0], 0.0, true[0] - x1)
            gap_y = max(y0 - true[1], 0.0, true[1] - y1)
            min_clearance = min(min_clearance, math.hypot(gap_x, gap_y))

        # the encoders report the commanded motion, with some noise, and know nothing of the extra turn
        d = v * dt * (1 + rng.normal(0, 0.05))
        dth = omega * dt * (1 + rng.normal(0, 0.05))
        odo[0] += d * math.cos(odo[2] + dth / 2)
        odo[1] += d * math.sin(odo[2] + dth / 2)
        odo[2] += dth
        kx.predict(d * math.cos(kth.x))
        ky.predict(d * math.sin(kth.x))
        kth.predict(dth)
        t += dt

        # a position and compass fix once per second
        if abs(t - round(t)) < 1e-6:
            kx.update(true[0] + rng.normal(0, 0.3))
            ky.update(true[1] + rng.normal(0, 0.3))
            kth.update(kth.x + wrap(true[2] + rng.normal(0, 0.05) - kth.x))

    final = (kx.x, ky.x) if use_estimator else (odo[0], odo[1])
    return {
        "reached": math.hypot(goal[0] - true[0], goal[1] - true[1]) < 0.5,
        "outcome": outcome,
        "time_s": round(t, 1),
        "replans": replans,
        "min_clearance_m": round(min_clearance, 2),
        "pose_error_m": round(math.hypot(true[0] - final[0], true[1] - final[1]), 2),
    }

result = run(seed=1)
print(result)
```

The result should show that the robot reached the goal, in about 27 seconds, with several replans, and a clearance from the obstacles that stays positive. Several of the replans happen because the scanner discovers the hidden box, or because new scan points change the map.

## 4. Which Parts Matter?

The best way to see the value of each component is to remove it. We run the full pipeline and a version in which the Kalman filter and its corrections are replaced by raw odometry, over six random seeds.

```python
def summarise(label, **kwargs):
    runs = [run(seed=s, **kwargs) for s in range(6)]
    reached = sum(r["reached"] for r in runs)
    print(f"{label:34s} reached {reached}/6   "
          f"mean time {np.mean([r['time_s'] for r in runs if r['reached']] or [float('nan')]):5.1f} s   "
          f"mean pose error {np.mean([r['pose_error_m'] for r in runs]):4.2f} m   "
          f"closest approach {min(r['min_clearance_m'] for r in runs):4.2f} m")
    return runs

full = summarise("with the Kalman filter", use_estimator=True)
odometry_only = summarise("odometry only (no corrections)", use_estimator=False)
print("outcomes without the estimator:", sorted({r["outcome"] for r in odometry_only}))
```

With the filter, all six runs reach the goal, in about 26 to 28 seconds, with a mean pose error of 0.18 metres. With odometry alone none of them do, and the pose error averages 3.85 metres. The unmodelled turn makes the heading estimate drift, so scans are placed in the wrong spot in the map, the map fills with spurious obstacles, the gap in the wall appears to close, and the planner finally reports that no path exists. The error in one component, the pose, has spread through the map, the plan and the commands, exactly as the architecture predicts.

This is a deliberately harsh test, since the odometry has a built-in bias. The aim is not that odometry is always useless, but that errors that the robot cannot see accumulate, and everything downstream suffers.

### 4.1 What about replanning?

The robot did not know about the box. Does the replanning loop help? Let us count how often the final path is a different one from the first.

```python
tracked = []
for s in range(3):
    r = run(seed=s)
    tracked.append((r["replans"], r["reached"]))
print("replans and success for 3 seeds:", tracked)
```

Replans of at least a few per run show that the robot constantly corrects its plan as the map grows. If you disable replanning, which is an interesting experiment, the robot follows the original path through the box, and collides. Try it: change the condition that triggers a replan so that it only fires if `path is None`.

## 5. The Pipeline as a ROS 2 Node

On a real robot the same structure is a node with subscribers for the sensors, a publisher for the commands, and a timer that drives the control loop. The code is shown for structure. It uses the functions of earlier weeks, `scan_to_occupancy` from Week 9, `a_star` from Week 13, and the controller of Week 10, and it needs a ROS 2 installation and a robot or simulator to run.

```py
import math
import rclpy
from rclpy.node import Node
from sensor_msgs.msg import LaserScan
from nav_msgs.msg import Odometry
from geometry_msgs.msg import Twist

class NavigationNode(Node):
    def __init__(self):
        super().__init__('navigation_node')
        self.scan_sub = self.create_subscription(LaserScan, '/scan', self.scan_cb, 10)
        self.odom_sub = self.create_subscription(Odometry, '/odom', self.odom_cb, 10)
        self.cmd_pub = self.create_publisher(Twist, '/cmd_vel', 10)
        self.timer = self.create_timer(0.2, self.control_loop)

        self.declare_parameter('goal_x', 9.0)
        self.declare_parameter('goal_y', 9.0)
        self.declare_parameter('max_speed', 0.3)

        self.path = None
        self.occupancy_grid = None
        self.current_pose = None

    def scan_cb(self, msg):
        if self.current_pose is None:
            return
        # Week 9: turn the scan into grid cells, using the current pose estimate
        self.occupancy_grid = scan_to_occupancy_pose(
            msg.ranges, msg.angle_min, msg.angle_increment, 100, 0.1, self.current_pose)

    def odom_cb(self, msg):
        p = msg.pose.pose
        yaw = 2.0 * math.atan2(p.orientation.z, p.orientation.w)     # planar yaw from a quaternion
        self.current_pose = (p.position.x, p.position.y, yaw)        # Week 12 filtering would go here

    def control_loop(self):
        if self.current_pose is None or self.occupancy_grid is None:
            return
        goal = (self.get_parameter('goal_x').value, self.get_parameter('goal_y').value)
        if self.path is None or self.path_blocked():
            self.path = a_star(self.occupancy_grid, self.current_pose[:2], goal)   # Week 13
        cmd = Twist()
        if self.path:
            cmd.linear.x, cmd.angular.z = self.follow(self.path)                   # Week 10
        self.cmd_pub.publish(cmd)           # an empty Twist stops the robot

    def path_blocked(self):
        return False                        # replace with a check of the next cells on the path

    def follow(self, path):
        return 0.0, 0.0                     # replace with the heading controller of the simulation

def main():
    rclpy.init()
    rclpy.spin(NavigationNode())
    rclpy.shutdown()
```

This is a skeleton, and two of its methods are left for you to fill in as part of the in-class exercise. Notice the things that a real system adds: a quaternion to yaw conversion, parameters for the goal and the speed limit, the check for missing data before the first scan arrives, and the fact that publishing an empty `Twist` stops the robot, which is the safe default.

## 6. In-Class Exercise

Assemble and run the minimal pipeline in simulation: drive the robot from a start to a goal position, while avoiding at least one simulated obstacle.

The code above does this. Use it as a starting point for the following tasks.

1. Run the pipeline for six seeds, and report the success count, time and closest approach.
2. Add a second hidden obstacle at a place of your choice, and check that the robot still reaches the goal.
3. Break one component at a time. Set the inflation to zero, remove the position fixes, or disable replanning. Describe in a sentence for each what failed first.

A way to make the experiments easy: give `run` extra keyword arguments such as `hidden=HIDDEN_BOX` and `inflation=3`, and use them where the code now uses the constants.

```python
def run_with_hidden(hidden_boxes, seed=1):
    """Same pipeline, with a different set of hidden obstacles."""
    global HIDDEN_BOX
    saved = HIDDEN_BOX
    HIDDEN_BOX = hidden_boxes
    try:
        return run(seed=seed)
    finally:
        HIDDEN_BOX = saved

print("no hidden box:       ", run_with_hidden([]))
print("two hidden boxes:    ", run_with_hidden([(7.0, 7.2, 8.0, 8.6), (2.5, 3.0, 3.3, 3.8)]))
```

The second box lies on the way to the gap, and the robot copes with it, with more replans. Move it so that it nearly closes the corridor beside the wall, for example to `(3.0, 3.0, 4.0, 4.2)`, and the planner reports that no path exists, because after inflation by 0.3 metres the remaining passage is too narrow. That is the honest result for a robot of that size, and a good illustration of why the safety margin and the robot radius have to be chosen with care.

Questions:

1. Which of the five steps of the loop is the slowest to compute? Would that matter on a real robot at 10 Hz?
2. What does the robot do if the goal is unreachable? How should it behave?
3. What is the safest behaviour when the estimator's uncertainty grows large?

## 7. Common Mistakes

1. Using the true pose in a simulation where the robot should have only the estimated one, which hides the problems.
2. Letting the timestamps of the scan and the pose differ, so that the scan is placed with a pose from a different moment.
3. Forgetting that a plan is out of date as soon as the map changes.
4. Running the planner in the control loop at the control rate, when planning at a lower rate would do.
5. Not handling the start-up period, when some topics have not yet produced data.
6. Not planning the failure case: no path, lost localisation, a blocked goal.
7. Mixing frames: a scan in the sensor frame, a pose in the odom frame, and a goal in the map frame.
8. Tuning individual components on their own, and then being surprised that they do not work well together.

## 8. Summary

A navigation system connects a map built from scans, a pose estimate from fused sensors, a global planner and a controller, with the pose estimate at the centre of everything. Errors do not stay where they arise: in our experiment, a small unmodelled turn, left uncorrected, corrupted the map until the planner found no way through, while the same robot with a simple Kalman filter reached the goal every time. Replanning lets the robot cope with a world that is not as the map says. Nav2 is the industrial version of this pipeline. Next week, in the last session, you present your capstone, review the course, and discuss the ethics and safety of robotics.

## 9. Practice Problems

1. Replace the proportional heading controller by the PID controller of Week 10, with angle wrapping, and compare the paths.
2. Replace A* by the RRT with shortcutting from Week 14, planning in continuous coordinates, and compare the time and the clearance.
3. Add a safety layer: if the nearest laser reading in front of the robot is under 0.3 metres, override the command with a stop. Decide where in the loop it belongs.
4. Log the pose error, the number of occupied cells and the replans over time, and plot them. What happens to each at the moments when the box is first seen?

## 10. Suggested Reading

1. The Nav2 documentation, in particular the concepts and configuration guides.
2. Macenski et al., "The Marathon 2: A Navigation System", IROS 2020.
3. Thrun, Burgard and Fox, Probabilistic Robotics, the introduction.

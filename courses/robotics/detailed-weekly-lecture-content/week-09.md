# Week 9: Midterm and Sensors II, Range Sensing

## Learning Objectives

By the end of this lecture, you should be able to:

1. Take the midterm with a sound strategy for the three kinds of question it contains.
2. Compare LiDAR, ultrasonic and infrared range sensors by range, precision and failure modes.
3. Interpret a 2D laser scan as ranges at evenly spaced angles.
4. Convert a scan, taken from a known robot pose, into an occupancy grid.
5. Improve the basic grid by marking free space as well as obstacles, and explain what a grid still cannot represent.

## 1. Midterm Exam

The midterm covers Weeks 1 to 8: coordinate transforms, kinematics, ROS 2, dynamics and actuators, and odometry. The review list at the end of Week 8 gives the topics, and the practice problems in each week give examples. The rest of this session introduces range sensing, which is needed for the second half of the course.

Three kinds of question usually appear.

1. Calculation questions, for example the position of an object in the world frame, the torque at a joint, or the pose after a few odometry steps. Write the formula, substitute with units, and give the result with units. Draw a small sketch with the frames labelled. Partial marks follow clear working.
2. Conceptual questions, for example "when would you use a service instead of a topic?" or "why does odometry drift?" Answer with a short, direct statement, followed by a reason and one concrete example.
3. Reading code, for example "what does this node do, and what will `ros2 topic hz` report?" Trace the code line by line, and note the period of any timer.

Practical advice: attempt the questions you are surest of first. Check angle units before you start any trigonometry. When a result looks implausible, such as a torque of a million N m or a robot that moves at 300 m/s, stop and look for a unit error before going on.

## 2. Range Sensors

Range sensors measure the distance to the nearest surface in some direction. They let a robot detect obstacles, and they give the external reference that corrects the drift of odometry.

### 2.1 LiDAR

A LiDAR emits laser pulses and measures the time they take to return, called time of flight, or the phase shift of a modulated beam. A 2D scanner rotates the beam, and returns a distance at each angle, so one revolution is a scan, a slice of the world at the height of the sensor. 3D scanners add more beams at different angles.

Strengths are accuracy, typically within a few centimetres, long range, from several metres to tens of metres, and a narrow beam, which gives good angular resolution. Weaknesses are cost, and trouble with glass and mirrors, which let the beam pass through or reflect it away, with black or very shiny surfaces, which return little light, and with strong sunlight, which adds noise.

### 2.2 Ultrasonic sensors

An ultrasonic sensor emits a burst of sound and times the echo. It is very cheap. The range is shorter, typically up to a few metres, and the beam is wide, perhaps 15 to 30 degrees, so the sensor knows that something is within a cone but not exactly where. Soft materials absorb sound, and angled surfaces can reflect the pulse away from the sensor. Several sensors operating at once can hear each other's echoes, which is called crosstalk.

### 2.3 Infrared range sensors

Infrared sensors measure distance from the intensity or the angle of the reflected infrared light. They are inexpensive and small, with a short range, usually well under a metre, and their output depends on the colour and reflectivity of the surface, so a black wall looks further away than a white wall at the same distance. They are used for close range detection, for instance as cliff sensors that stop a robot from driving off a step.

```python
sensors = {
    "LiDAR (2D)":  {"range_m": (0.1, 12.0), "beam_deg": 0.5,  "typical_error_m": 0.02, "weakness": "glass, mirrors, black surfaces"},
    "Ultrasonic":  {"range_m": (0.02, 4.0), "beam_deg": 20.0, "typical_error_m": 0.01, "weakness": "wide beam, soft or angled surfaces"},
    "Infrared":    {"range_m": (0.04, 0.8), "beam_deg": 5.0,  "typical_error_m": 0.02, "weakness": "depends on surface colour"},
}

# how wide is the beam footprint at a distance of 3 m?
import math
for name, s in sensors.items():
    near, far = s["range_m"]
    if 3.0 <= far:
        width = 2 * 3.0 * math.tan(math.radians(s["beam_deg"] / 2))
        print(f"{name:12s} footprint at 3 m: {width:5.2f} m wide")
    else:
        print(f"{name:12s} cannot see 3 m (max range {far} m)")
```

The ultrasonic beam spreads to about a metre at 3 metres, while the LiDAR footprint is about 3 centimetres. This is the main reason why LiDAR is preferred for mapping, while ultrasonic sensors are used for simple obstacle warnings.

(The figures in this table are typical values chosen for teaching, not the specifications of any product. Always read the data sheet of the sensor you actually use.)

## 3. The LaserScan Message

ROS 2 describes a 2D LiDAR scan with the message type `sensor_msgs/LaserScan`. Its main fields are:

1. `angle_min` and `angle_max`: the first and last beam angles, in radians, in the sensor frame.
2. `angle_increment`: the angle between consecutive beams.
3. `ranges`: the array of distances in metres, one per beam.
4. `range_min` and `range_max`: the valid distances. A beam that hits nothing within range is reported as infinity, or as `nan` if the reading is invalid.

The angle of beam `i` is `angle_min + i * angle_increment`, and a reading is a point in the sensor frame at `(r cos a, r sin a)`.

### 3.1 A simulated LiDAR

For the labs we use a simulator, and for this lecture we use a small simulation of our own, so that you can generate scans anywhere. The room is a set of wall segments. For each beam we find the nearest segment that the beam hits.

```python
import numpy as np

def raycast(origin, angle, segments, max_range):
    """Distance along a ray to the nearest wall segment, or inf if nothing is hit within max_range."""
    d = np.array([np.cos(angle), np.sin(angle)])
    p = np.asarray(origin, dtype=float)
    best = np.inf
    for q1, q2 in segments:
        q1, q2 = np.asarray(q1, float), np.asarray(q2, float)
        s = q2 - q1
        denom = d[0] * s[1] - d[1] * s[0]            # cross product of the two directions
        if abs(denom) < 1e-12:
            continue                                  # parallel
        w = q1 - p
        t = (w[0] * s[1] - w[1] * s[0]) / denom       # distance along the ray
        u = (w[0] * d[1] - w[1] * d[0]) / denom       # position along the segment
        if t >= 0 and 0 <= u <= 1 and t < best:
            best = t
    return best if best <= max_range else np.inf

def simulate_scan(pose, segments, angle_min=-np.pi / 2, angle_max=np.pi / 2, n_beams=181,
                  max_range=8.0, noise_std=0.02, seed=0):
    rng = np.random.default_rng(seed)
    x, y, theta = pose
    angles = np.linspace(angle_min, angle_max, n_beams)
    ranges = np.array([raycast((x, y), theta + a, segments, max_range) for a in angles])
    ranges = ranges + rng.normal(0, noise_std, size=n_beams)       # inf plus noise stays inf
    return angles, ranges

# a 10 m by 8 m room with a 1 m box in the middle
room = [((0, 0), (10, 0)), ((10, 0), (10, 8)), ((10, 8), (0, 8)), ((0, 8), (0, 0))]
box = [((4, 3), (5, 3)), ((5, 3), (5, 4)), ((5, 4), (4, 4)), ((4, 4), (4, 3))]
walls = room + box

robot_pose = (2.0, 3.5, 0.0)               # facing along +x, towards the box
angles, ranges = simulate_scan(robot_pose, walls)
finite = np.isfinite(ranges)
print("beams:", len(ranges), " hits within range:", int(finite.sum()))
print("range straight ahead (expected about 2.0 m to the box): %.2f m" % ranges[len(ranges) // 2])
print("range at -90 deg (to the wall at y = 0, expected 3.5 m): %.2f m" % ranges[0])
```

The beam straight ahead hits the face of the box, which is 2.0 metres away, and the beam at minus 90 degrees points to the right, toward the wall at `y = 0`, 3.5 metres away. The checks on those two readings confirm that the simulator behaves as we expect. The beams that look beyond the box towards the far wall at `x = 10` are 8 metres away, that is at `max_range`, so some of them report infinity.

## 4. From Range Scan to Occupancy Grid

A scan is a list of numbers, but a planner wants a map. An occupancy grid divides the world into square cells, and records for each one whether it is occupied. It is the standard representation for 2D navigation, and it is what the planners of Weeks 13 and 14 will search.

The first version, as in the original lecture, marks the cell where each beam ends.

```python
def scan_to_occupancy(ranges, angle_min, angle_increment, grid_size, resolution, robot_pos):
    grid = np.zeros((grid_size, grid_size))
    for i, r in enumerate(ranges):
        if r == float('inf') or np.isnan(r):
            continue
        angle = angle_min + i * angle_increment
        x = robot_pos[0] + r * np.cos(angle)
        y = robot_pos[1] + r * np.sin(angle)
        gx, gy = int(x / resolution), int(y / resolution)
        if 0 <= gx < grid_size and 0 <= gy < grid_size:
            grid[gy, gx] = 1  # mark occupied
    return grid
```

Look carefully, because this function makes an assumption that is easy to miss. It adds the angle of the beam to the robot's position, but it never uses the robot's heading. It is correct only when the robot is facing along the x axis, and the scan is already in the world's orientation. For a robot at an arbitrary heading, each beam's world angle is `theta + angle`. This is exactly the frame conversion of Week 2: a point measured in the sensor frame is moved to the world frame using the robot's pose. A version that does it properly takes the whole pose.

```python
def scan_to_occupancy_pose(ranges, angle_min, angle_increment, grid_size, resolution, robot_pose):
    x0, y0, theta = robot_pose
    grid = np.zeros((grid_size, grid_size))
    for i, r in enumerate(ranges):
        if not np.isfinite(r):
            continue
        angle = theta + angle_min + i * angle_increment      # the heading is added here
        x = x0 + r * np.cos(angle)
        y = y0 + r * np.sin(angle)
        gx, gy = int(x / resolution), int(y / resolution)
        if 0 <= gx < grid_size and 0 <= gy < grid_size:
            grid[gy, gx] = 1
    return grid

resolution = 0.1                       # each cell is 10 cm
grid_size = 100                        # 100 x 100 cells covers 10 m x 10 m
grid = scan_to_occupancy_pose(ranges, angles[0], angles[1] - angles[0], grid_size, resolution, robot_pose)
print("occupied cells:", int(grid.sum()))

# the box face at x = 4 should show up as occupied cells around column 40, rows 30 to 40
face = grid[30:40, 39:41]
print("occupied cells on the box face:", int(face.sum()))
```

Note the convention: `grid[row, column]` corresponds to `grid[gy, gx]`, which is easy to reverse. The first index is the y direction.

### 4.1 Seeing the grid

```python
import matplotlib.pyplot as plt

plt.imshow(grid, origin="lower", cmap="gray_r", extent=[0, grid_size * resolution, 0, grid_size * resolution])
plt.scatter([robot_pose[0]], [robot_pose[1]], color="gray", marker="^", label="robot")
plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.title("Occupied cells from one scan")
plt.legend()
plt.show()
```

You will see the face of the box, and the far wall, as a line of dots, with gaps between them, because the beams are spaced by one degree, and further away they are further apart than the cell size. This is the first limit of marking only the endpoints, and it gets worse with distance.

### 4.2 Marking free space

The grid so far has two states, empty and occupied, but empty is ambiguous. A cell that no beam reached may be free, or may simply be unseen. A beam that ends 4 metres away also tells us that the cells along the way are free, since otherwise the beam would have stopped earlier. A good grid has three states: unknown, free and occupied. We obtain the free cells by stepping along each beam from the robot to its end point.

```python
UNKNOWN, FREE, OCCUPIED = -1, 0, 1

def build_grid(ranges, angle_min, angle_increment, grid_size, resolution, robot_pose, max_range=8.0):
    x0, y0, theta = robot_pose
    grid = np.full((grid_size, grid_size), UNKNOWN, dtype=int)
    for i, r in enumerate(ranges):
        hit = np.isfinite(r)
        reach = r if hit else max_range
        angle = theta + angle_min + i * angle_increment
        # walk along the beam in half-cell steps, marking free space
        for s in np.arange(0.0, reach - resolution, resolution / 2):
            gx = int((x0 + s * np.cos(angle)) / resolution)
            gy = int((y0 + s * np.sin(angle)) / resolution)
            if 0 <= gx < grid_size and 0 <= gy < grid_size and grid[gy, gx] != OCCUPIED:
                grid[gy, gx] = FREE
        if hit:
            gx = int((x0 + r * np.cos(angle)) / resolution)
            gy = int((y0 + r * np.sin(angle)) / resolution)
            if 0 <= gx < grid_size and 0 <= gy < grid_size:
                grid[gy, gx] = OCCUPIED
    return grid

full = build_grid(ranges, angles[0], angles[1] - angles[0], grid_size, resolution, robot_pose)
print("unknown:", int((full == UNKNOWN).sum()), " free:", int((full == FREE).sum()), " occupied:", int((full == OCCUPIED).sum()))

plt.imshow(full, origin="lower", cmap="gray_r", extent=[0, 10, 0, 10], vmin=-1.5, vmax=1)
plt.title("Unknown (light), free (white), occupied (dark)")
plt.show()
```

Most cells remain unknown. The robot sees only a half circle in front of it, and cannot see behind the box. One scan from one place is not a map, and that is why a real robot combines many scans as it moves, with a good estimate of its pose for each. This is mapping, and it relies on all the earlier topics of the course.

Also notice the box. The beams stop at its front face, and so the cells behind it stay unknown. A grid does not claim that there is nothing behind the box, only that it has not seen anything there.

### 4.3 A second scan from a different place

Combining scans is simple when the poses are known. Take a second scan from the other side of the box, and merge the results, so that a cell becomes occupied if either scan saw it occupied, and free if one saw it free and none saw it occupied.

```python
def merge(g1, g2):
    out = np.full_like(g1, UNKNOWN)
    out[(g1 == FREE) | (g2 == FREE)] = FREE
    out[(g1 == OCCUPIED) | (g2 == OCCUPIED)] = OCCUPIED
    return out

pose2 = (7.5, 3.5, np.pi)             # on the far side of the box, facing back towards it
angles2, ranges2 = simulate_scan(pose2, walls, seed=1)
grid2 = build_grid(ranges2, angles2[0], angles2[1] - angles2[0], grid_size, resolution, pose2)
both = merge(full, grid2)
print("unknown after one scan: ", int((full == UNKNOWN).sum()))
print("unknown after two scans:", int((both == UNKNOWN).sum()))
```

The unknown area shrinks, and the back face of the box is now seen too. In practice grids store a probability for every cell, updated with each observation in log-odds form, so that a few noisy readings do not flip a cell between states. We use the simple three-state grid in this course.

## 5. In-Class Exercise

Given a simulated LiDAR scan (an array of ranges and angles) and a known robot pose, build the occupancy grid with the function above, and visualize it as a 2D image.

Use these steps.

1. Generate a scan from the pose `(4.5, 6.0, -pi/2)`, which faces towards the box from above.
2. Build the grid with `scan_to_occupancy_pose` and with `build_grid`.
3. Check that the box face is where you expect it, using the pose and the geometry of the room.

```python
pose = (4.5, 6.0, -np.pi / 2)
a3, r3 = simulate_scan(pose, walls, seed=2)
g3 = scan_to_occupancy_pose(r3, a3[0], a3[1] - a3[0], grid_size, resolution, pose)
ahead = r3[len(r3) // 2]
print("range straight ahead: %.2f m (the top face of the box at y = 4 is 2.0 m below the robot)" % ahead)
print("occupied cells:", int(g3.sum()))

plt.imshow(g3, origin="lower", cmap="gray_r", extent=[0, 10, 0, 10])
plt.scatter([pose[0]], [pose[1]], marker="v", color="gray")
plt.title("Scan from (4.5, 6) looking down")
plt.show()
```

Questions:

1. What happens to the grid if you call the original `scan_to_occupancy` with a robot pose that has a heading of `pi / 2`? Try it, and describe the error.
2. How does the resolution of the grid change the answer? Try `resolution = 0.05` and `0.2`, and look at the gaps between the points.
3. How much does a heading error of 3 degrees in the robot pose move the far wall of the room in the map? (Remember the lesson of odometry drift, and compute `8 * tan(3 degrees)`.)

## 6. Common Mistakes

1. Ignoring the robot heading when converting a scan into the world frame.
2. Mixing up `grid[row, column]` and `(x, y)` order.
3. Treating `inf` as a large distance, so that it is plotted as an obstacle, instead of as "nothing seen".
4. Treating empty cells as free when they are really unknown.
5. Using angle units of degrees in the trigonometry.
6. Forgetting that the world is not static, so that old occupied cells can be stale.
7. Trusting a single scan from an uncertain pose.

## 7. Summary

LiDAR, ultrasonic and infrared sensors measure distance, with different costs, ranges and weaknesses. A 2D scan is an array of ranges at evenly spaced angles, and turning it into points in the world uses the same frame transforms as Week 2. An occupancy grid records occupied, free and unknown cells, and the basic version of the scan-to-grid function must include the robot's heading to be correct. A grid built from one scan is incomplete, and this is why robots combine many. In the next weeks we turn to control, vision, and state estimation, which together let the robot use what it senses.

## 8. Practice Problems

1. Add a second obstacle to the room, and check that the scan from the starting pose sees it.
2. Write a function `nearest_obstacle(ranges, angles)` that returns the distance and the bearing of the closest hit, and use it to implement a simple stop rule at 0.5 m.
3. Convert the occupancy grid into an inflated grid, in which every occupied cell also marks its neighbours within the robot's radius, so that a planner can treat the robot as a point.
4. Simulate a robot that moves 0.5 m between scans, merges five scans, and shows how the unknown area shrinks.

## 9. Suggested Reading

1. Thrun, Burgard and Fox, Probabilistic Robotics, the chapter on occupancy grid mapping.
2. The documentation of `sensor_msgs/LaserScan` in ROS 2.

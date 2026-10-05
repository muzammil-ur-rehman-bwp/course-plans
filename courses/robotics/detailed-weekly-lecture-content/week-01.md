# Week 1: Introduction to Robotics

## Learning Objectives

By the end of this lecture, you should be able to:

1. Define a robot and place a given robot in the course taxonomy.
2. Describe the sense-plan-act architecture and the role of feedback.
3. Distinguish reactive and deliberative control, and explain why most real robots combine both.
4. Name the tools used in this course and what each one does.
5. Check your own environment, and run a small simulation of a robot's control loop in plain Python.

## 1. What Is a Robot?

Ask ten engineers for a definition of a robot and you will get at least five answers. A vacuum cleaner that moves by itself is clearly a robot to most people, and a washing machine that follows a programmed cycle is not, though both are machines run by software. The difference most people feel is that a robot senses its environment and acts on it, and that its actions depend on what it senses.

For this course we use the following working definition. A robot is a programmable physical system that senses its environment and acts upon it to accomplish a task. All three words in the definition matter. Programmable means its behaviour is set by software. Physical means it has a body, with all the mess that implies: friction, noise, delays and limited power. Sensing and acting together mean that there is a loop between the world and the machine.

### 1.1 A taxonomy of robots

The following categories are used throughout the course.

1. Manipulators, or robot arms. They have a fixed base and a chain of links connected by joints, and an end-effector such as a gripper or a tool. Industrial pick and place arms and welding robots are typical.
2. Mobile robots. They move through an environment. Wheeled and tracked ground robots are the most common, and the warehouse robots that carry shelves are an example.
3. Aerial robots, or drones. They operate in three dimensional airspace and have to fight gravity all the time, which changes how they are controlled.
4. Humanoid robots. They are bipedal and shaped like people. They are mechanically and computationally demanding, since balance is a hard control problem.

There are others, such as underwater vehicles and soft robots, but the four above cover most of what we will meet. This course focuses mainly on mobile robots with differential drive, which means two independently driven wheels, and on simple planar arms. These two are enough to teach the central ideas, and both can be simulated without any hardware.

### 1.2 Why simulate?

Real robots break, cost money, and need space and a person to reset them after each failed experiment. A simulator lets us test an idea hundreds of times in a minute, and it lets us make mistakes without consequence. The price is that a simulation is only an approximation of the world. Behaviour that works in simulation can fail on hardware because of unmodelled friction, sensor noise or delays. This gap is called the sim to real gap. It is not a reason to avoid simulation, but it is a reason to test on hardware before trusting any result.

## 2. The Sense-Plan-Act Architecture

The classical way of organising a robot's software is a pipeline with a feedback loop.

```
Sensors --> Perception/State Estimation --> Planning --> Control --> Actuators
   ^                                                                     |
   +---------------------------------------------------------------------+
                         (feedback loop)
```

1. Sense. Read raw data from sensors such as wheel encoders, an inertial measurement unit (IMU), a camera or a LiDAR.
2. Perceive and estimate. Turn raw numbers into a useful description of the world and of the robot's own state: where am I, and what is around me?
3. Plan. Decide what to do, for instance by computing a path to a goal.
4. Control. Convert the plan into commands for the motors, such as wheel speeds.
5. Act. The actuators execute the commands, which changes the world, which the sensors then observe. This closes the loop.

The loop matters because the world does not do what we expect. A wheel slips, a person walks across the path, a battery weakens. With feedback the robot sees the discrepancy and corrects it. A system without feedback is called open loop, and it works only if the world behaves exactly as predicted.

### 2.1 Reactive and deliberative control

There are two broad styles of connecting sensing to action.

1. Deliberative control builds a model of the world, plans a sequence of actions using that model, and then executes the plan. It can handle tasks that need foresight, such as finding a route through a building. It is slow when the world changes, and it relies on the model being good.
2. Reactive control maps the current sensor reading directly to an action, with no explicit planning and little or no memory. An example is a rule such as "if something is closer than 30 centimetres, turn away". It is fast and robust, but it cannot reason about the future, so it can be trapped by a dead end.

Most real systems use both. A common design has a slow planner that chooses a route, and a fast reactive layer that avoids obstacles and keeps the robot safe while it follows the route. The capstone pipeline in Week 15 follows the same pattern.

### 2.2 A first simulation: the control loop in code

The following code is a tiny simulated robot living on a line. It senses the distance to a wall ahead, and it must stop before reaching it. We run the same world with an open loop controller and with a feedback controller, and the difference between them shows why feedback matters. Nothing here needs special libraries.

```python
import random

def simulate(controller, steps=120, wall=10.0, gain=1.1, seed=1):
    """A 1D robot. Commands are speeds. The wheels really move 'gain' times
    faster than commanded (a calibration error the planner does not know about)."""
    rng = random.Random(seed)
    position = 0.0
    history = []
    for t in range(steps):
        measured_distance = wall - position + rng.gauss(0, 0.02)   # sense
        speed = controller(measured_distance, t)                   # plan and control
        actual = speed * gain * (1 + rng.uniform(-0.05, 0.05))     # act, imperfectly
        position += actual * 0.1                                   # dt = 0.1 s
        history.append(position)
    return history

def open_loop(distance, t):
    # plan: drive at 1 m/s for 95 steps (9.5 m, which stops just before the wall).
    # It ignores the sensor.
    return 1.0 if t < 95 else 0.0

def feedback(distance, t):
    # a proportional controller: slow down in proportion to the remaining distance
    # aim to stop 0.5 m before the wall
    return max(0.0, 1.0 * (distance - 0.5))

for name, ctrl in [("open loop", open_loop), ("feedback", feedback)]:
    final = simulate(ctrl)[-1]
    gap = 10.0 - final
    note = "  <- passed through the wall" if gap < 0 else ""
    print(f"{name:10s} final position {final:5.2f}  distance to wall {gap:5.2f}{note}")
```

Run it. The open loop plan is perfectly sensible on paper, 95 steps at 1 metre per second stops the robot 0.5 metres before the wall. But the real wheels turn 10 percent faster than commanded, so the robot travels about 10.4 metres and goes through the wall. The planner never found out, because it never looked. The feedback controller does not know about the calibration error either, but it keeps measuring the distance and slows down as the wall approaches, so it stops close to 0.5 metres, and it behaves the same way for every random seed. Try other seeds and other values of `gain`, such as 0.8 or 1.3, and compare the two. This is the kind of design decision we make again and again in the next weeks.

## 3. Sense, Plan, Act in a Slightly Bigger Example

Here is a more complete reactive and deliberative combination, in a grid world. A planner finds a path on a known map, and a reactive layer overrides it when an unexpected obstacle appears. You will recognise the breadth first search from the Programming for AI course.

```python
from collections import deque

GRID = [
    "S.......",
    ".##.##..",
    ".#....#.",
    ".#.##.#.",
    "...#...G",
]

def find(grid, ch):
    for r, row in enumerate(grid):
        if ch in row:
            return (r, row.index(ch))

def plan_path(grid, start, goal, blocked=frozenset()):
    """Deliberative layer: breadth-first search on the known map."""
    frontier = deque([(start, [start])])
    seen = {start}
    while frontier:
        cell, path = frontier.popleft()
        if cell == goal:
            return path
        r, c = cell
        for dr, dc in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            nr, nc = r + dr, c + dc
            if (0 <= nr < len(grid) and 0 <= nc < len(grid[0])
                    and grid[nr][nc] != "#" and (nr, nc) not in blocked and (nr, nc) not in seen):
                seen.add((nr, nc))
                frontier.append(((nr, nc), path + [(nr, nc)]))
    return None

start, goal = find(GRID, "S"), find(GRID, "G")
path = plan_path(GRID, start, goal)
print("planned path length:", len(path) - 1)       # 11 steps

# the world has a hidden obstacle that the map does not show
hidden_obstacle = path[4]            # a cell on the planned route
print("hidden obstacle at:", hidden_obstacle)

pos = start
walked = [pos]
known_blocked = set()
i = 1
while pos != goal:
    nxt = path[i]
    if nxt == hidden_obstacle:                 # sense: bump sensor says blocked
        known_blocked.add(nxt)                 # reactive layer: stop and report
        path = plan_path(GRID, pos, goal, frozenset(known_blocked))   # replan
        print("obstacle detected at", nxt, "-> replanned, new length", len(path) - 1)
        i = 1
        continue
    pos = nxt
    walked.append(pos)
    i += 1

print("reached goal:", pos == goal, " steps walked:", len(walked) - 1)
```

The robot plans 11 steps. After three steps it finds the next cell blocked, and it replans. The new route from its current cell is 8 steps, so it still reaches the goal in 11 steps in total, because an equally short alternative existed. The sequence is: plan, execute, sense a surprise, react, replan. In a real robot the "bump sensor" would be a LiDAR scan or a camera, and the replanning would be done by a navigation stack, but the structure is the same.

## 4. Course Tooling Overview

1. Python, with `rclpy`. The scripting language for our robot programs. The package `rclpy` is the Python client library for ROS 2.
2. ROS 2. A middleware that connects sensors, planners and actuators as independent programs called nodes, which exchange messages through topics and services.
3. Gazebo and Webots. Physics simulators that let us test robot behaviour without hardware.
4. OpenCV. A computer vision library for processing camera images, used in Week 11.

A note on how this document handles code. Where the real tools are needed, the lectures show the ROS 2 code, which you run on a machine that has ROS 2 installed. Because not every student will have that from day one, most lectures also include a plain Python version that exercises the same ideas using only NumPy and Matplotlib, so that you can run and test it anywhere. Your lab sessions use the real tools.

## 5. Environment Setup

1. Verify Python 3.10 or newer and a ROS 2 installation, using `ros2 --version` for the second.
2. Launch a simulator, Gazebo or Webots, and check that it opens a default scene.
3. Source the ROS 2 environment in every new terminal session used for this course. For example `source /opt/ros/<distro>/setup.bash`, with the name of your ROS 2 distribution in place of the placeholder. Forgetting this is the commonest cause of "command not found" errors.

The script below checks the Python parts and tells you whether ROS 2's Python library is visible. It runs even if ROS 2 is missing, and reports it as not found.

```python
import sys
import shutil

print("Python:", sys.version.split()[0])
if sys.version_info < (3, 8):
    print("  warning: this course expects a recent Python (3.10 or newer)")

for lib in ["numpy", "matplotlib"]:
    try:
        module = __import__(lib)
        print(f"{lib:12s} {module.__version__}")
    except ImportError:
        print(f"{lib:12s} NOT INSTALLED  (pip install {lib})")

try:
    import rclpy
    print("rclpy        found (ROS 2 Python library is available)")
except ImportError:
    print("rclpy        not found (source your ROS 2 setup file, or install ROS 2)")

print("ros2 command:", shutil.which("ros2") or "not on PATH")
print("gazebo / webots:", shutil.which("gz") or shutil.which("gazebo") or shutil.which("webots") or "not on PATH")
```

If `rclpy` is missing after you have installed ROS 2, you almost certainly have not sourced the setup file in this terminal.

## 6. Worked Example: Classifying Robots

Here is a way to turn the in-class exercise into data you can query. We describe five robots by category, typical sensors and control style, and then ask a few questions of the table.

```python
robots = [
    {"name": "Warehouse shelf carrier", "category": "mobile",
     "sensors": ["wheel encoders", "LiDAR", "camera"], "control": "deliberative with reactive safety layer"},
    {"name": "Robot vacuum cleaner", "category": "mobile",
     "sensors": ["bumper", "cliff sensors", "wheel encoders", "dust sensor"], "control": "mostly reactive"},
    {"name": "Welding arm", "category": "manipulator",
     "sensors": ["joint encoders", "force sensor"], "control": "deliberative (pre-planned trajectory)"},
    {"name": "Quadcopter", "category": "aerial",
     "sensors": ["IMU", "barometer", "GPS", "camera"], "control": "fast reactive stabilisation plus planner"},
    {"name": "Biped humanoid", "category": "humanoid",
     "sensors": ["IMU", "joint encoders", "foot force sensors", "camera"], "control": "reactive balance plus deliberative gait planning"},
]

for r in robots:
    print(f"{r['name']:26s} {r['category']:12s} sensors: {len(r['sensors'])}  control: {r['control']}")

print()
uses_imu = [r["name"] for r in robots if "IMU" in r["sensors"]]
print("robots that rely on an IMU:", uses_imu)
```

Note that for several robots the honest answer to "reactive or deliberative" is "both". A good answer to the exercise says which layer dominates and why. A drone cannot wait for a planner before correcting a tilt, so the stabilising loop is reactive and fast, while the choice of waypoints is slow and deliberative.

## 7. In-Class Exercise

For five example robots, identify the taxonomy category, the primary sensors likely used, and whether its control is better described as reactive or deliberative. Use the table above as a model, and choose robots that are not in it, for example a self-driving car, a robotic lawn mower, a surgical robot, a delivery drone and a robot arm that sorts parcels.

Questions for discussion:

1. Which of your robots could work with no planning at all, and what would it be unable to do?
2. Which of them operate in an environment that can change faster than the planner can react?
3. Pick one, and describe what the feedback loop is. What is measured, what is commanded, and how quickly?

## 8. Common Mistakes

1. Describing a robot only by what it does, without saying what it senses. Without a sense there is no robot, only a machine.
2. Assuming an open loop plan will work in the real world because it worked in simulation.
3. Treating reactive and deliberative control as an either-or choice.
4. Forgetting to source the ROS 2 environment in a new terminal.
5. Starting to write robot code before checking that the simulator itself runs.

## 9. Summary

A robot senses, decides and acts in a loop, and feedback is what makes it robust to a world that does not match its model. Control can be reactive, deliberative or a mixture, and most practical systems are a mixture. This course uses Python, ROS 2, a simulator and OpenCV, and begins with the mathematics of coordinate frames, which is the subject of next week.

## 10. Practice Problems

1. Change the feedback controller in section 2.2 so that it stops at 1.0 metre from the wall. Measure how much the final position varies over 20 different random seeds, for both controllers.
2. Add a sensor noise level of 0.2 metres to the simulation. Does the feedback controller still work? What could you do about it?
3. In the grid world example, make the hidden obstacle block the only route to the goal. What should the robot do?
4. For a robot of your choice, draw the sense-plan-act diagram and label each box with a concrete component.

## 11. Suggested Reading

1. Siegwart, Nourbakhsh and Scaramuzza, Introduction to Autonomous Mobile Robots, the first chapter.
2. Siciliano and Khatib (editors), Springer Handbook of Robotics, the introduction.
3. The official ROS 2 documentation, section on installation for your platform.

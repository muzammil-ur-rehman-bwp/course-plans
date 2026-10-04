# Week 1 — Lecture Content: Introduction to Robotics

## 1. What Is a Robot?
A robot is a programmable physical system that senses its environment and acts upon it to
accomplish a task. Key taxonomy used throughout this course:
- **Manipulators** (robot arms) — fixed base, articulated joints, e.g., industrial pick-and-place
  arms.
- **Mobile robots** — move through an environment, e.g., wheeled/tracked ground robots.
- **Aerial robots (drones)** — operate in 3D airspace.
- **Humanoid robots** — bipedal, human-like form factor.

This course focuses primarily on mobile robots (differential-drive) and simple manipulators
(planar arms), simulated via ROS 2 + Gazebo/Webots.

## 2. The Sense-Plan-Act Architecture
```
Sensors --> Perception/State Estimation --> Planning --> Control --> Actuators
   ^                                                                     |
   +---------------------------------------------------------------------+
                         (feedback loop)
```
- **Sense**: read raw sensor data (encoders, IMU, camera, LiDAR).
- **Plan**: decide what to do (e.g., compute a path to a goal).
- **Act**: execute motor commands to carry out the plan.

This is the deliberative model; purely **reactive** robots skip explicit planning and map
sensing directly to action (e.g., a simple obstacle-avoidance behavior). Most real systems,
and this course's capstone pipeline (Week 15), blend both.

## 3. Course Tooling Overview
| Tool | Role |
|---|---|
| Python (`rclpy`) | Scripting language for ROS 2 nodes |
| ROS 2 | Middleware connecting sensors, planners, and actuators via nodes/topics/services |
| Gazebo / Webots | Physics simulator — lets us test robot behavior without hardware |
| OpenCV | Computer vision processing on camera data (Week 11) |

## 4. Environment Setup
1. Verify Python 3.10+ and ROS 2 installation (`ros2 --version`).
2. Launch a simulator (Gazebo/Webots) and confirm it opens a default scene.
3. Source the ROS 2 environment (`source /opt/ros/<distro>/setup.bash` or equivalent) in every
   new terminal session used for this course.

## 5. In-Class Exercise
For 5 example robots, identify: taxonomy category, primary sensors likely used, and whether its
control is better described as reactive or deliberative.

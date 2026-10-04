# Week 6 Lecture Plan — Robotics
## Topic: ROS 2 Continued — Services, Parameters, Simulation

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the difference between topics and services. (*Understand*)
2. Apply ROS 2 services, parameters, and launch files. (*Apply*)
3. Apply `/cmd_vel` commands to drive a simulated robot in Gazebo/Webots. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Services vs. topics | Request/response pattern, live demo |
| 0:25–0:50 | Parameters & launch files | Declaring parameters, writing a launch file |
| 0:50–1:00 | Break | — |
| 1:00–1:30 | Simulator bring-up | Launching a simulated differential-drive robot in Gazebo/Webots |
| 1:30–2:00 | Driving the simulated robot | Publishing `/cmd_vel` commands from a ROS 2 node |

### Materials/Equipment
- ROS 2, Gazebo/Webots, simulated robot model (instructor-provided)

### Formative Check (in-class)
Exercise: write a simple service that returns whether a given coordinate is within the robot's
workspace bounds.

### Link to Lab/Assessment
Lab 6: launch the simulated robot and drive it through a simple predefined path via `/cmd_vel`.

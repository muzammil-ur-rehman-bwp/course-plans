# Lab Manual 6 — Services, Launch Files & Simulation Bring-Up

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Write a ROS 2 service, a launch file, and drive a simulated robot via `/cmd_vel`.

## Setup
Continue in `lab05_pkg` or start `lab06_pkg`; use the instructor-provided simulated robot
model/world.

## Procedure
1. **Task A — Service:** implement the workspace-bounds-check service described in lecture.
2. **Task B — Launch file:** write a launch file that starts the Lab 5 publisher/subscriber
   together with this lab's service node.
3. **Task C — Simulator bring-up:** launch the provided simulated differential-drive robot in
   Gazebo/Webots.
4. **Task D — Driving the robot:** write a node that publishes a sequence of `/cmd_vel` commands
   to drive the robot in a square path.

## Expected Output
A working package with all nodes + launch file; a short video/screenshot sequence showing the
robot completing the square path in simulation.

## Submission
Submit the package + report by the end of the lab session.

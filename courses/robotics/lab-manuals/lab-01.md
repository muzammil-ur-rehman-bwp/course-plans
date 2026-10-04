# Lab Manual 1 — Environment Setup & ROS 2 Hello World

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Verify the Python/ROS 2/simulator environment and run a first ROS 2 node.

## Setup
1. Verify `python3 --version` (3.10+), `ros2 --version`.
2. Launch Gazebo/Webots and confirm the default scene opens.

## Procedure
1. **Task A — Environment check:** run the instructor-provided verification script/commands and
   screenshot/paste the successful output.
2. **Task B — ROS 2 hello world:** create a package and run a minimal node that logs "Hello,
   Robotics!" every second using `self.get_logger().info(...)`.
3. **Task C — CLI exploration:** with the node running, use `ros2 node list` and `ros2 node info
   <node_name>` to inspect it; paste the output.
4. **Task D — Taxonomy exercise:** for 5 instructor-provided example robots, classify each by
   taxonomy (manipulator/mobile/aerial/humanoid) and primary sensing modality.

## Expected Output
A short report (`lab01_report.md`) with Tasks A–D results, plus the working ROS 2 package.

## Submission
Submit the package + `lab01_report.md` by the end of the lab session.

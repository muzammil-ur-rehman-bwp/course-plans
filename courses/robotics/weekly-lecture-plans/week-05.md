# Week 5 Lecture Plan — Robotics
## Topic: Introduction to ROS 2

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the ROS 2 node/topic/message architecture. (*Understand*)
2. Apply `rclpy` to write a publisher and subscriber node. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | ROS 2 architecture | Nodes, topics, messages, DDS middleware (conceptual) |
| 0:25–0:55 | Workspaces & packages | `colcon build`, package structure, live demo |
| 0:55–1:05 | Break | — |
| 1:05–1:40 | Writing a publisher/subscriber | Live-coded minimal `rclpy` publisher + subscriber |
| 1:40–2:00 | Inspecting with CLI tools | `ros2 topic list/echo/hz`, live demo |

### Materials/Equipment
- ROS 2 installation, `rclpy`
- Starter package skeleton

### Formative Check (in-class)
Exercise: modify the demo publisher to publish a different message type/rate, and verify with
`ros2 topic hz`.

### Link to Lab/Assessment
Lab 5: write a ROS 2 package with a custom publisher and subscriber node.

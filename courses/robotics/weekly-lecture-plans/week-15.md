# Week 15 Lecture Plan — Robotics
## Topic: Autonomous Navigation Pipeline

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain how perception, state estimation, planning, and control integrate into a navigation
   pipeline. (*Understand*)
2. Analyze and assemble a minimal end-to-end navigation pipeline in simulation. (*Analyze*)
3. Evaluate the pipeline's performance on a localize-plan-drive-while-avoiding-obstacle task.
   (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Pipeline integration | How Weeks 8–14 pieces connect into one system (diagram) |
| 0:25–0:55 | The ROS 2 Navigation Stack (Nav2) | Overview as the production-grade analog |
| 0:55–1:05 | Break | — |
| 1:05–2:00 | Assembly walkthrough | Live-build a minimal pipeline: sense → estimate → plan → drive |

### Materials/Equipment
- ROS 2, Gazebo/Webots, OpenCV (if vision used)

### Formative Check (in-class)
Exercise: trace through the pipeline diagram and identify which Week's lecture/lab produced each
component (sensing, estimation, planning, control).

### Link to Lab/Assessment
Lab 15: assemble and test an end-to-end navigation pipeline in simulation. **Assignment 3** due
at the start of this week.

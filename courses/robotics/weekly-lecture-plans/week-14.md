# Week 14 Lecture Plan — Robotics
## Topic: Path Planning II — Sampling-Based Planning & Obstacle Avoidance

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the limitations of grid search in high-dimensional/continuous spaces. (*Understand*)
2. Apply a basic RRT planner to a 2D planning problem. (*Apply*)
3. Analyze and compare RRT vs. A* paths on the same problem. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Limits of grid search | Curse of dimensionality (conceptual) |
| 0:25–1:00 | RRT concept | Random sampling + tree growth, live-coded basic RRT |
| 1:00–1:10 | Break | — |
| 1:10–1:35 | Local obstacle avoidance | Reactive methods (potential field or bug algorithm, conceptual) |
| 1:35–2:00 | Comparison | RRT vs. A* paths on the same obstacle map |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib

### Formative Check (in-class)
Exercise: run the RRT implementation with different sample counts and observe path quality vs.
runtime tradeoff.

### Link to Lab/Assessment
Lab 14: implement a basic RRT planner and compare it against the Week 13 A* planner.

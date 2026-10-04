# Week 13 Lecture Plan — Robotics
## Topic: Path Planning I — Grid-Based Planning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain configuration space and the occupancy grid representation for planning. (*Understand*)
2. Apply A* search to plan a path on an occupancy grid. (*Apply*)
3. Analyze planned paths for optimality and sensitivity to heuristic choice. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Configuration space | Mapping the physical world to a planning representation |
| 0:25–0:55 | Grid-based search for planning | Reframing A* (from PAI-course-style search) for navigation |
| 0:55–1:05 | Break | — |
| 1:05–1:40 | Worked example | Plan a path across a grid with obstacles using A* |
| 1:40–2:00 | Path visualization | Overlay planned path on the occupancy grid |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib

### Formative Check (in-class)
Exercise: plan a path between two points on a provided occupancy grid; compare paths using two
different heuristics.

### Link to Lab/Assessment
Lab 13: implement A* on an occupancy grid and visualize the planned path.
**Assignment 3 assigned this week** (path planning), due start of Week 15.

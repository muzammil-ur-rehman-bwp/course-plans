# Week 2 Lecture Plan — Robotics
## Topic: Math Foundations — Frames & Transformations

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain coordinate frames and why robots need multiple frames. (*Understand*)
2. Apply rotation matrices and homogeneous transforms to convert points between frames. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why coordinate frames? | World frame vs. robot frame vs. sensor frame, diagram |
| 0:25–0:55 | Rotation matrices | 2D rotation matrix derivation, live NumPy demo |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Homogeneous transforms | Combining rotation + translation into a single 3x3/4x4 matrix |
| 1:35–2:00 | Composing transforms | Chaining transforms across multiple frames, worked example |

### Materials/Equipment
- Live-coding environment, NumPy
- Diagram handout: frame composition example

### Formative Check (in-class)
Exercise: given a point in a sensor frame and the sensor's pose relative to the robot, compute
the point's coordinates in the robot frame.

### Link to Lab/Assessment
Lab 2: implement rotation/translation matrices and transform points between frames in NumPy.

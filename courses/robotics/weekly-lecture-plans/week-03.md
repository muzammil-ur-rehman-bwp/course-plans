# Week 3 Lecture Plan — Robotics
## Topic: Forward Kinematics

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain degrees of freedom and joint types. (*Understand*)
2. Apply forward kinematics to compute end-effector position for a planar arm. (*Apply*)
3. Apply the differential-drive kinematic model to update a mobile robot's pose. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | DOF & joint types | Revolute vs. prismatic, counting DOF examples |
| 0:20–0:55 | Forward kinematics (arm) | 2-/3-link planar arm derivation, live NumPy demo |
| 0:55–1:05 | Break | — |
| 1:05–1:40 | Differential-drive model | Wheel velocities → (x, y, theta) update equations |
| 1:40–2:00 | Worked example | Simulate a short trajectory for a differential-drive robot |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib
- Diagram handout: 2-link arm geometry; differential-drive wheel geometry

### Formative Check (in-class)
Exercise: compute end-effector position for a given set of joint angles by hand, then verify
with code.

### Link to Lab/Assessment
Lab 3: implement forward kinematics for a 2-link arm and simulate a differential-drive robot's
pose over a sequence of wheel velocity commands.

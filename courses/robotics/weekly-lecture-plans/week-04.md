# Week 4 Lecture Plan — Robotics
## Topic: Inverse Kinematics

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the inverse kinematics problem and why it can have multiple/no solutions. (*Understand*)
2. Apply analytical IK to a 2-link planar arm. (*Apply*)
3. Analyze the reachable workspace of a given arm geometry. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | The IK problem | Multiple solutions (elbow up/down), unreachable targets |
| 0:25–0:55 | Analytical IK (2-link arm) | Law-of-cosines derivation, live NumPy demo |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Numerical/iterative IK (conceptual) | Jacobian-based approach, when analytical IK isn't feasible |
| 1:35–2:00 | Workspace visualization | Plotting reachable workspace for a given arm |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib

### Formative Check (in-class)
Exercise: given a target (x, y), compute the two possible joint-angle solutions (elbow up/down)
for a 2-link arm.

### Link to Lab/Assessment
Lab 4: implement analytical IK for a 2-link arm and visualize its reachable workspace.
**Assignment 1 assigned this week** (kinematics), due start of Week 6.

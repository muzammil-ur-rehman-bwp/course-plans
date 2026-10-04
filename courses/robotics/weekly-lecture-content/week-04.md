# Week 4 — Lecture Content: Inverse Kinematics

## 1. The Inverse Kinematics (IK) Problem
Given a desired end-effector position, find the joint angles that achieve it. Unlike forward
kinematics (one input → one output), IK can have **multiple solutions** (e.g., "elbow up" vs.
"elbow down" for a 2-link arm), or **no solution** if the target is outside the reachable
workspace.

## 2. Analytical IK for a 2-Link Planar Arm
Using the law of cosines on the triangle formed by the two links and the target point:
```python
import numpy as np

def inverse_kinematics_2link(x, y, l1, l2):
    d = np.sqrt(x**2 + y**2)
    if d > l1 + l2 or d < abs(l1 - l2):
        return None  # target unreachable

    cos_theta2 = (d**2 - l1**2 - l2**2) / (2 * l1 * l2)
    theta2 = np.arccos(np.clip(cos_theta2, -1.0, 1.0))  # elbow-down solution
    theta2_alt = -theta2                                 # elbow-up solution

    k1 = l1 + l2 * np.cos(theta2)
    k2 = l2 * np.sin(theta2)
    theta1 = np.arctan2(y, x) - np.arctan2(k2, k1)

    return (theta1, theta2), theta2_alt  # elbow-down angles + the alternate elbow-up theta2
```
The `d > l1 + l2` and `d < abs(l1 - l2)` checks implement the reachability test directly.

## 3. Numerical/Iterative IK (Conceptual)
For arms with more joints or more complex geometry, analytical solutions become impractical.
Jacobian-based iterative methods instead: start from a guess, compute the Jacobian (how small
joint changes affect end-effector position), and iteratively nudge joint angles toward the
target — conceptually similar to gradient descent (Week 10 of the PAI course, for comparison).

## 4. Workspace Visualization
```python
import matplotlib.pyplot as plt

xs, ys = [], []
for t1 in np.linspace(0, 2*np.pi, 100):
    for t2 in np.linspace(-np.pi, np.pi, 100):
        x, y = forward_kinematics_2link(t1, t2, l1=1.0, l2=0.8)
        xs.append(x); ys.append(y)
plt.scatter(xs, ys, s=1)
```
Sweeping all joint-angle combinations through forward kinematics traces out the reachable
workspace — a useful sanity check before attempting IK on a given target.

## 5. In-Class Exercise
For a given 2-link arm and target point, compute both IK solutions (elbow up/down) by hand, then
verify with `inverse_kinematics_2link`; identify one target point that is unreachable and explain
why using the `d` calculation.

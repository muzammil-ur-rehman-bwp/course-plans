# Week 3 — Lecture Content: Forward Kinematics

## 1. Degrees of Freedom & Joint Types
- **Revolute joint**: rotates about an axis (an angle, in radians/degrees).
- **Prismatic joint**: slides linearly (a distance).
- **Degrees of Freedom (DOF)**: the number of independent parameters needed to fully describe a
  system's configuration — a 2-link planar arm with 2 revolute joints has 2 DOF.

## 2. Forward Kinematics: 2-Link Planar Arm
Given joint angles `theta1, theta2` and link lengths `l1, l2`, the end-effector position is:
```python
import numpy as np

def forward_kinematics_2link(theta1, theta2, l1, l2):
    x1, y1 = l1 * np.cos(theta1), l1 * np.sin(theta1)
    x2 = x1 + l2 * np.cos(theta1 + theta2)
    y2 = y1 + l2 * np.sin(theta1 + theta2)
    return x2, y2
```
This directly uses the rotation/transform machinery from Week 2 — each link's end position is
the previous joint's position plus a rotated offset.

## 3. Differential-Drive Kinematic Model
A differential-drive robot has two independently driven wheels. Given left/right wheel
velocities `v_l, v_r`, wheel separation `L`, the robot's linear velocity `v` and angular velocity
`omega` are:
```
v = (v_r + v_l) / 2
omega = (v_r - v_l) / L
```
Pose update (discrete time step `dt`):
```python
def update_pose(x, y, theta, v, omega, dt):
    x_new = x + v * np.cos(theta) * dt
    y_new = y + v * np.sin(theta) * dt
    theta_new = theta + omega * dt
    return x_new, y_new, theta_new
```
This is the same equation ROS 2's odometry computation uses internally (we implement a simple
version by hand here before relying on the framework in later weeks).

## 4. Worked Example
Simulate a robot driving in a circle by setting constant `v_l, v_r` with `v_l != v_r`, applying
`update_pose` repeatedly, and plotting the resulting trajectory.

## 5. In-Class Exercise
Given joint angles for a 2-link arm, compute the end-effector position by hand, then verify with
`forward_kinematics_2link`. Separately, simulate 10 steps of differential-drive motion with given
wheel velocities and plot the path.

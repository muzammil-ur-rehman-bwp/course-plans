# Week 2 — Lecture Content: Math Foundations — Frames & Transformations

## 1. Why Coordinate Frames?
A robot involves multiple reference frames: the **world frame** (fixed, global), the **robot
frame** (moves with the robot's base), and **sensor frames** (attached to each sensor, which may
be offset from the robot's base). Converting measurements between frames is a prerequisite for
almost everything else in the course (odometry, kinematics, sensor fusion).

## 2. Rotation Matrices
A 2D rotation by angle `theta`:
```python
import numpy as np

def rotation_matrix_2d(theta):
    c, s = np.cos(theta), np.sin(theta)
    return np.array([[c, -s],
                      [s,  c]])

R = rotation_matrix_2d(np.pi / 4)
point = np.array([1, 0])
rotated = R @ point
```
3D rotations extend this idea (about x/y/z axes); this course uses 2D rotations primarily, since
the mobile-robot labs operate in a 2D ground plane.

## 3. Homogeneous Transforms
Combining rotation and translation into a single matrix lets us compose transforms by simple
matrix multiplication:
```python
def homogeneous_transform_2d(theta, tx, ty):
    R = rotation_matrix_2d(theta)
    T = np.eye(3)
    T[:2, :2] = R
    T[:2, 2] = [tx, ty]
    return T

def transform_point(T, point_2d):
    p = np.array([point_2d[0], point_2d[1], 1.0])
    return (T @ p)[:2]
```

## 4. Composing Transforms
If `T_world_robot` describes the robot's pose in the world frame, and `T_robot_sensor` describes
a sensor's pose relative to the robot, then a point measured in the sensor frame converts to the
world frame via:
```python
T_world_sensor = T_world_robot @ T_robot_sensor
point_world = transform_point(T_world_sensor, point_in_sensor_frame)
```
This chaining pattern recurs throughout the course (e.g., converting a LiDAR detection into
world coordinates for path planning in Week 13).

## 5. In-Class Exercise
Given `T_world_robot` (robot pose) and a point measured in the sensor frame with a known
`T_robot_sensor` offset, compute the point's world-frame coordinates.

# Week 2: Math Foundations, Frames and Transformations

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain why a robot needs several coordinate frames, and name the common ones.
2. Build 2D rotation matrices and homogeneous transforms, and apply them to points.
3. Compose transforms by matrix multiplication and invert them correctly.
4. Convert a sensor measurement into world coordinates, by hand and in code.
5. Avoid the usual mistakes: wrong multiplication order, degrees mixed with radians, and angle wrap-around.

## 1. Why Coordinate Frames?

A robot with a laser scanner on its head sees an obstacle three metres ahead. Is the obstacle three metres ahead of the robot's wheels? Of the scanner? Of some fixed point in the room? Each answer is correct in its own frame, and they are all different numbers. A robot uses several reference frames at the same time.

1. The world frame, also called the global or map frame. It is fixed, and does not move when the robot moves.
2. The robot frame, or base frame. It is attached to the robot's body and moves with it. A common convention puts the origin between the wheels, the x axis pointing forward and the y axis pointing left.
3. Sensor frames. Each sensor has its own frame, usually offset from the robot's base. A camera mounted 20 centimetres forward and 30 centimetres up reports positions relative to itself.

Nearly everything that follows depends on converting between frames. Odometry (Week 8) tells us how the robot frame moves in the world frame. A range scan (Week 9) arrives in the sensor frame and has to be put on a map in the world frame. A robot arm's gripper position (Weeks 3 and 4) is found by moving from one link's frame to the next. If you understand transforms well, a great deal of robotics becomes bookkeeping.

A habit that prevents many errors is to always name the frame a quantity is in. Writing `p_world` or `p_sensor` costs nothing and will save you hours.

## 2. Rotation Matrices

A rotation by an angle `theta` about the origin, in two dimensions, is a matrix.

```
R(theta) = [ cos(theta)  -sin(theta) ]
           [ sin(theta)   cos(theta) ]
```

Multiplying a point by this matrix rotates it anticlockwise by `theta`. Angles are in radians in all code from here on.

```python
import numpy as np

def rotation_matrix_2d(theta):
    c, s = np.cos(theta), np.sin(theta)
    return np.array([[c, -s],
                     [s,  c]])

R = rotation_matrix_2d(np.pi / 4)
point = np.array([1, 0])
rotated = R @ point
print(rotated.round(4))        # [0.7071 0.7071]
```

Check this against geometry. The point `(1, 0)` lies on the x axis, and turning it 45 degrees anticlockwise puts it at `(cos 45, sin 45)`, which is `(0.7071, 0.7071)`.

Rotation matrices have some useful properties that you can test directly.

```python
R = rotation_matrix_2d(0.6)

print("R^T R = I:        ", np.allclose(R.T @ R, np.eye(2)))
print("inverse is R^T:   ", np.allclose(np.linalg.inv(R), R.T))
print("determinant is 1: ", np.isclose(np.linalg.det(R), 1.0))
print("lengths preserved:", np.isclose(np.linalg.norm(R @ [3, 4]), 5.0))
print("angles add:       ", np.allclose(rotation_matrix_2d(0.2) @ rotation_matrix_2d(0.4), rotation_matrix_2d(0.6)))
```

All five should print `True`. The inverse of a rotation is simply its transpose, which is cheaper and more accurate than a general matrix inverse, and means that undoing a rotation is the same as rotating by the opposite angle.

Three dimensional rotations follow the same idea, with a 3 by 3 matrix for rotation about each axis. We work in two dimensions in this course, since the mobile robot labs operate on a flat ground plane. In three dimensions the order of rotations matters, and rotations do not commute, which makes them harder. In two dimensions they do commute, which is a luxury.

### 2.1 Degrees, radians and wrapping

Most bugs in beginner robot code are about angles.

```python
import math

print(np.cos(90))                   # wrong: NumPy expects radians
print(np.cos(math.radians(90)))     # about 6e-17, effectively zero
print(math.degrees(np.pi / 3))      # 60.0 degrees
```

The first line prints about -0.448, not zero, because NumPy treated 90 as radians. Convert with `math.radians` or `np.deg2rad`.

Angles also wrap around. A heading of 359 degrees and one of minus 1 degree are the same direction, but their numerical difference is 360, which would look like a huge error to a controller. We normalise angle differences to the range from minus pi to pi.

```python
def wrap_angle(a):
    """Map any angle in radians to the interval [-pi, pi)."""
    return (a + np.pi) % (2 * np.pi) - np.pi

target = np.deg2rad(350)
current = np.deg2rad(10)
print("naive error (deg):  ", np.rad2deg(target - current))                  # 340
print("wrapped error (deg):", np.rad2deg(wrap_angle(target - current)))      # -20
```

The wrapped value says the robot should turn 20 degrees clockwise, which is the sensible answer. We use `wrap_angle` again in the PID controller of Week 10.

## 3. Homogeneous Transforms

A rotation alone cannot move the origin. To describe a general change of frame, we need a rotation and a translation, which is `p' = R p + t`. This is an affine operation, and it cannot be written as a single matrix multiplication of the point, so chaining several of them is awkward.

The trick is to add an extra coordinate. We write the 2D point `(x, y)` as the vector `(x, y, 1)`. This is a homogeneous coordinate. Then rotation and translation together fit in a single 3 by 3 matrix.

```
T = [ cos(theta)  -sin(theta)   tx ]
    [ sin(theta)   cos(theta)   ty ]
    [      0            0        1 ]
```

and the transformed point is `T @ [x, y, 1]`.

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

T = homogeneous_transform_2d(np.pi / 2, 2.0, 1.0)
print(T.round(3))
print(transform_point(T, [1.0, 0.0]).round(3))     # [2. 2.]
```

The point `(1, 0)` is rotated by 90 degrees to `(0, 1)`, and then translated by `(2, 1)`, which gives `(2, 2)`. Notice the order: rotate first, then translate. The transform `T` is the pose of a frame, and reading its columns tells you where the frame's axes point and where its origin lies.

### 3.1 What a transform means

The notation matters, so read it carefully. We write `T_world_robot` for the transform that takes a point expressed in the robot frame and gives its coordinates in the world frame. The same matrix also describes the robot's pose: its translation part is the robot's position in the world, and its rotation part is the robot's heading. One matrix, two interpretations, and the two are always consistent.

A good mnemonic is that the subscripts cancel like units: `T_world_robot @ p_robot` gives `p_world`, because `robot` next to `robot` cancels.

### 3.2 Inverting a transform

If `T_world_robot` converts robot coordinates to world coordinates, its inverse converts the other way, from world to robot. We can use `np.linalg.inv`, but there is a neater formula that avoids a general inverse. For `T = [R t; 0 1]`, the inverse is `[R^T  -R^T t; 0 1]`.

```python
def invert_transform(T):
    R, t = T[:2, :2], T[:2, 2]
    Tinv = np.eye(3)
    Tinv[:2, :2] = R.T
    Tinv[:2, 2] = -R.T @ t
    return Tinv

T = homogeneous_transform_2d(0.7, 3.0, -1.0)
print(np.allclose(invert_transform(T), np.linalg.inv(T)))   # True
print(np.allclose(T @ invert_transform(T), np.eye(3)))      # True

p_robot = np.array([1.0, 2.0])
p_world = transform_point(T, p_robot)
back = transform_point(invert_transform(T), p_world)
print(p_world.round(3), back.round(3))
```

The round trip gets us back to the original point, which is a good check that a transform has been implemented correctly. Use this sort of test regularly. Frame bugs rarely crash the program, they just put things in the wrong place, so tests that check consistency are valuable.

## 4. Composing Transforms

If `T_world_robot` gives the robot's pose in the world, and `T_robot_sensor` gives the sensor's pose relative to the robot, then the sensor's pose in the world is their product.

```
T_world_sensor = T_world_robot @ T_robot_sensor
point_world = transform_point(T_world_sensor, point_in_sensor_frame)
```

Look at the subscripts. `world_robot` followed by `robot_sensor` gives `world_sensor`. The matching inner names cancel. This chaining pattern is used throughout the course, for example to convert a LiDAR detection into world coordinates for path planning in Week 13.

The order matters, because matrix multiplication is not commutative. `T_robot_sensor @ T_world_robot` is a meaningless product here, and the code would still run and give a wrong answer. Check it.

```python
A = homogeneous_transform_2d(np.pi / 2, 1.0, 0.0)
B = homogeneous_transform_2d(0.0, 2.0, 0.0)
print("A @ B applied to the origin:", transform_point(A @ B, [0, 0]).round(3))
print("B @ A applied to the origin:", transform_point(B @ A, [0, 0]).round(3))
```

The first gives `(1, 2)` and the second gives `(3, 0)`. In `A @ B` the point is first moved by `B` (two units along x) and then rotated and moved by `A`. The rightmost transform acts first on the point.

## 5. Worked Example: A Detected Obstacle in World Coordinates

A robot stands at `(2, 1)` in the world, and its heading is 90 degrees, which means it faces along the positive y axis. A laser scanner is mounted 0.2 metres ahead of the robot's centre, facing the same direction as the robot. The scanner detects an obstacle at `(3, 0)` in the scanner frame, which is 3 metres straight ahead of it.

Where is the obstacle in the world?

By hand:

1. The scanner is 0.2 metres ahead of the robot along the robot's x axis. In the world, the robot's x axis points along world y, so the scanner is at `(2, 1.2)`, and faces along the world y axis.
2. An obstacle 3 metres straight ahead of the scanner is therefore at `(2, 1.2 + 3) = (2, 4.2)`.

Now by code, using the transforms.

```python
T_world_robot = homogeneous_transform_2d(np.deg2rad(90), 2.0, 1.0)
T_robot_sensor = homogeneous_transform_2d(0.0, 0.2, 0.0)

T_world_sensor = T_world_robot @ T_robot_sensor
point_in_sensor_frame = [3.0, 0.0]
point_world = transform_point(T_world_sensor, point_in_sensor_frame)

print("sensor position in world:", T_world_sensor[:2, 2].round(3))
print("obstacle in world:       ", point_world.round(3))
```

The output should match the hand calculation, `(2, 1.2)` for the sensor and `(2, 4.2)` for the obstacle. If your own hand calculation disagrees with the code, draw a picture before suspecting either.

### 5.1 A whole scan at once

A LiDAR returns many readings, each a distance at some angle. Each one is a point in the sensor frame, `(r cos(a), r sin(a))`. Because NumPy handles arrays, we can transform all of them at once with a single matrix multiplication.

```python
def scan_to_world(ranges, angles, T_world_sensor):
    pts = np.stack([ranges * np.cos(angles),
                    ranges * np.sin(angles),
                    np.ones_like(ranges)])           # shape (3, N): homogeneous points
    return (T_world_sensor @ pts)[:2].T              # shape (N, 2)

angles = np.deg2rad(np.arange(-45, 46, 15))          # seven beams from -45 to +45 degrees
ranges = np.array([3.0, 3.2, 3.5, 3.0, 3.5, 3.2, 3.0])
world_pts = scan_to_world(ranges, angles, T_world_sensor)
print(world_pts.round(2))
```

The beams fan out ahead of the scanner, which faces along the world y axis, so all the points should have a larger y than the sensor. You can plot them to confirm.

```python
import matplotlib.pyplot as plt

plt.scatter(world_pts[:, 0], world_pts[:, 1], color="black", label="scan points")
plt.scatter(*T_world_robot[:2, 2], color="gray", marker="s", label="robot")
plt.scatter(*T_world_sensor[:2, 2], color="gray", marker="^", label="sensor")
plt.axis("equal")
plt.xlabel("world x (m)")
plt.ylabel("world y (m)")
plt.legend()
plt.title("Laser scan points in the world frame")
plt.show()
```

## 6. In-Class Exercise

Given `T_world_robot` (the robot's pose) and a point measured in the sensor frame, with a known `T_robot_sensor` offset, compute the point's world-frame coordinates.

Try this variation. The robot is at `(-1, 4)` with a heading of 30 degrees. The sensor is mounted 0.3 metres behind the robot's centre and 0.1 metres to its left, rotated 180 degrees (it faces backwards). It measures a point at `(2, 0.5)` in the sensor frame.

1. Write down `T_robot_sensor` in terms of its rotation and translation. Note that behind the robot means a negative x offset, and left means a positive y offset.
2. Predict roughly where the point lies in the world. Is it ahead of or behind the robot?
3. Compute it with code and compare with your estimate.

```python
T_world_robot = homogeneous_transform_2d(np.deg2rad(30), -1.0, 4.0)
T_robot_sensor = homogeneous_transform_2d(np.deg2rad(180), -0.3, 0.1)
p_sensor = [2.0, 0.5]

p_robot = transform_point(T_robot_sensor, p_sensor)
p_world = transform_point(T_world_robot @ T_robot_sensor, p_sensor)
print("in the robot frame:", p_robot.round(3))     # [-2.3, -0.4]
print("in the world frame:", p_world.round(3))
```

The point is 2.3 metres behind the robot and 0.4 metres to its right, which is consistent with a sensor that faces backwards. In the world it is behind the robot's heading direction, so it ends up near `(-2.8, 2.5)`.

## 7. Common Mistakes

1. Multiplying transforms in the wrong order. Check with the subscript cancelling rule.
2. Passing degrees to `np.sin` and `np.cos`.
3. Forgetting to append the 1 to make a point homogeneous, or appending 0 for a point. A 0 in the last place treats the vector as a direction, which is rotated but not translated, and is sometimes what you want, but only when you intend it.
4. Subtracting angles without wrapping them.
5. Inverting a transform by negating its translation. The correct inverse is `-R^T t`, not `-t`.
6. Not naming the frame of a variable, so that later nobody knows whether it is `p_sensor` or `p_world`.

## 8. Summary

A robot lives in several frames, and almost every calculation involves moving a quantity from one to another. A rotation matrix handles orientation, and a homogeneous transform adds translation so that chains of frames become matrix products. The order of multiplication is essential, the inverse of a transform has a simple formula, and angles need care with units and wrapping. Next week we use these tools to describe how a robot arm and a wheeled robot move.

## 9. Practice Problems

1. Show numerically that rotating a point by 30 degrees and then by 50 degrees is the same as rotating it by 80 degrees.
2. A camera is mounted at `(0.25, 0)` on a robot, facing forward. The robot is at `(1, 1)` with heading 45 degrees. A cone is seen at `(2, -0.5)` in the camera frame. Where is it in the world?
3. Write a function `relative_pose(T_world_a, T_world_b)` that returns `T_a_b`, the pose of frame `b` as seen from frame `a`.
4. Generate a circular arc of 100 scan points in the sensor frame, transform them with three different robot poses, and plot all three clouds on the same axes.

## 10. Suggested Reading

1. Siegwart, Nourbakhsh and Scaramuzza, Introduction to Autonomous Mobile Robots, the section on coordinate transformations.
2. Lynch and Park, Modern Robotics, the chapter on configuration space and rigid body motions, free online.

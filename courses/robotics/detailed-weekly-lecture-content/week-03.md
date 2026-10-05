# Week 3: Forward Kinematics

## Learning Objectives

By the end of this lecture, you should be able to:

1. Define degrees of freedom, and distinguish revolute from prismatic joints.
2. Compute the end-effector position of a 2-link planar arm from its joint angles.
3. Show that forward kinematics is the same as chaining the transforms of Week 2.
4. Write the kinematic model of a differential-drive robot and update its pose over time.
5. Simulate a robot driving in a circle, check the result against theory, and see how the time step affects accuracy.

## 1. Degrees of Freedom and Joint Types

Kinematics describes motion geometrically, without asking what forces cause it. Forward kinematics answers a direct question: given the settings of all the joints of a mechanism, where does it end up?

A joint connects two links and allows one kind of relative motion between them. The two basic types are:

1. Revolute joint. It rotates about an axis, like a hinge or an elbow. Its value is an angle, in radians or degrees.
2. Prismatic joint. It slides along a line, like a piston or a drawer. Its value is a distance.

The number of degrees of freedom (DOF) is the number of independent parameters needed to describe the configuration of a system completely. A 2-link planar arm with two revolute joints has 2 DOF, since two angles fix the arm's shape. A point moving on a plane has 2 DOF, its x and y coordinates. A rigid body on a plane has 3: two position coordinates and one orientation angle. A rigid body in space has 6.

The count has practical consequences. To place a gripper at any position in a plane you need at least 2 DOF. To place it at any position and orientation in a plane you need at least 3. An arm with fewer DOF than the task needs cannot do the task, and one with more has redundancy, which gives flexibility but makes control more complicated (Week 4).

## 2. Forward Kinematics of a 2-Link Planar Arm

Let the first link have length `l1` and rotate at the base by an angle `theta1`, measured from the x axis. The second link has length `l2` and is attached at the end of the first. Its angle `theta2` is measured relative to the first link, so the second link points at the absolute angle `theta1 + theta2`.

The elbow is at `(l1 cos theta1, l1 sin theta1)`. The end-effector is the elbow plus the second link's offset.

```python
import numpy as np

def forward_kinematics_2link(theta1, theta2, l1, l2):
    x1, y1 = l1 * np.cos(theta1), l1 * np.sin(theta1)
    x2 = x1 + l2 * np.cos(theta1 + theta2)
    y2 = y1 + l2 * np.sin(theta1 + theta2)
    return x2, y2

x, y = forward_kinematics_2link(np.deg2rad(30), np.deg2rad(45), 1.0, 0.8)
print(round(x, 4), round(y, 4))
```

A hand calculation, for `theta1 = 30`, `theta2 = 45` degrees, `l1 = 1`, `l2 = 0.8`:

1. Elbow: `x1 = cos 30 = 0.8660`, `y1 = sin 30 = 0.5000`.
2. The second link points at 75 degrees. `0.8 cos 75 = 0.8 * 0.2588 = 0.2071`, and `0.8 sin 75 = 0.8 * 0.9659 = 0.7727`.
3. End-effector: `x2 = 0.8660 + 0.2071 = 1.0731`, `y2 = 0.5 + 0.7727 = 1.2727`.

The code should print `1.0731 1.2727`.

### 2.1 Drawing the arm

A picture helps to catch mistakes. This function returns the three joint positions, so that we can draw the arm as two line segments.

```python
import matplotlib.pyplot as plt

def arm_points(theta1, theta2, l1, l2):
    p0 = (0.0, 0.0)
    p1 = (l1 * np.cos(theta1), l1 * np.sin(theta1))
    p2 = forward_kinematics_2link(theta1, theta2, l1, l2)
    return [p0, p1, p2]

for t1, t2, style in [(30, 45, "-"), (30, -45, "--"), (90, 90, ":")]:
    pts = np.array(arm_points(np.deg2rad(t1), np.deg2rad(t2), 1.0, 0.8))
    plt.plot(pts[:, 0], pts[:, 1], style, marker="o", color="black", label=f"theta1={t1}, theta2={t2}")

plt.axis("equal")
plt.grid(True, alpha=0.3)
plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.legend()
plt.title("A 2-link arm in three configurations")
plt.show()
```

Look at the first two configurations. They share the same `theta1` and elbow position, and differ only in whether the second link bends up or down. These are different end-effector positions. We will meet the reverse situation next week: one end-effector position reached by two different joint settings.

### 2.2 Forward kinematics as chained transforms

The formula above is not a special trick. It is the Week 2 machinery applied link by link. Each link contributes a rotation by its joint angle followed by a translation along the link by its length. Multiplying these homogeneous transforms from the base to the tip gives the pose of the end-effector.

```python
def rot(theta):
    c, s = np.cos(theta), np.sin(theta)
    return np.array([[c, -s, 0], [s, c, 0], [0, 0, 1]])

def trans(x, y):
    T = np.eye(3)
    T[:2, 2] = [x, y]
    return T

def fk_by_transforms(thetas, lengths):
    T = np.eye(3)
    for theta, length in zip(thetas, lengths):
        T = T @ rot(theta) @ trans(length, 0.0)    # rotate at the joint, then move along the link
    return T

T = fk_by_transforms([np.deg2rad(30), np.deg2rad(45)], [1.0, 0.8])
print("position:", T[:2, 2].round(4))
print("tip orientation (deg):", round(np.rad2deg(np.arctan2(T[1, 0], T[0, 0])), 1))
```

The position matches the previous calculation, and the same code works for any number of links, which is its advantage. It also gives the orientation of the end-effector for free: the tip points at `theta1 + theta2 = 75` degrees.

## 3. Differential-Drive Kinematic Model

A differential-drive robot has two wheels on a common axis, each driven independently, and usually a caster that just balances the body. It is the most common design for small indoor mobile robots, because it is simple, cheap and able to turn in place.

Let the left and right wheel linear speeds be `v_l` and `v_r`, and let the distance between the wheels be `L`. The robot's forward speed is the mean of the two, and its rate of turning depends on their difference.

```
v     = (v_r + v_l) / 2
omega = (v_r - v_l) / L
```

Some cases build intuition.

1. `v_l = v_r`: omega is zero and the robot drives straight.
2. `v_l = -v_r`: v is zero and the robot spins on the spot.
3. `v_r > v_l > 0`: the robot curves to the left, which is anticlockwise.
4. `v_l = 0, v_r > 0`: the robot pivots about its left wheel.

To track the robot over time we update its pose, `(x, y, theta)`, in small time steps `dt`.

```python
def update_pose(x, y, theta, v, omega, dt):
    x_new = x + v * np.cos(theta) * dt
    y_new = y + v * np.sin(theta) * dt
    theta_new = theta + omega * dt
    return x_new, y_new, theta_new

def wheels_to_body(v_l, v_r, L):
    return (v_r + v_l) / 2.0, (v_r - v_l) / L

v, omega = wheels_to_body(0.4, 0.6, 0.5)
print("v =", v, "m/s   omega =", omega, "rad/s")
```

This is the same equation that ROS 2's odometry computations use internally. We write it by hand first, so that later, when we use the framework, it is clear what it is doing.

The update is an approximation. It moves the robot along its current heading for a whole time step, and only afterwards turns it, while in reality it moves along a gentle arc. With small enough steps the error is small. We will measure this below.

## 4. Worked Example: Driving in a Circle

If the wheel speeds are constant, and unequal, the robot must travel in a circle. The radius follows from the model: `R = v / omega`.

With `v_l = 0.4`, `v_r = 0.6` and `L = 0.5`:

1. `v = 0.5 m/s` and `omega = (0.6 - 0.4) / 0.5 = 0.4 rad/s`.
2. The radius is `R = 0.5 / 0.4 = 1.25 m`.
3. One full circle needs `2 pi / omega = 15.7 s`.

We simulate it.

```python
v_l, v_r, L = 0.4, 0.6, 0.5
v, omega = wheels_to_body(v_l, v_r, L)

dt = 0.05
T_total = 2 * np.pi / omega
steps = int(round(T_total / dt))

x, y, theta = 0.0, 0.0, 0.0
xs, ys = [x], [y]
for _ in range(steps):
    x, y, theta = update_pose(x, y, theta, v, omega, dt)
    xs.append(x)
    ys.append(y)

print(f"radius expected {v / omega:.3f} m")
print(f"centre of the circle expected at (0, {v / omega:.3f})")
print(f"after one period the robot is at ({x:.3f}, {y:.3f})  (it started at (0, 0))")

plt.plot(xs, ys, color="black")
plt.scatter([0], [0], color="gray", zorder=3, label="start")
plt.axis("equal")
plt.grid(True, alpha=0.3)
plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.title("Differential-drive robot with v_l = 0.4 and v_r = 0.6")
plt.legend()
plt.show()
```

The robot finishes very close to the starting point, within a few millimetres. The small gap exists because 15.7 seconds is not a whole number of 0.05 second steps, so the loop stops a fraction of a step short.

Is a closed lap a good test of accuracy? Not really, and it is worth knowing why. With a constant turning rate, the heading advances by the same angle each step, so the Euler steps are equal vectors rotated evenly around the circle, and they sum to zero over a full lap whatever the step size. The path closes, even though it is slightly the wrong size. A better test compares the position with the exact answer at some point along the way. The exact position after a time `t` on this circle is `(R sin(omega t), R (1 - cos(omega t)))`. At a quarter lap this is `(R, R)`.

```python
def quarter_lap_error(dt, update=update_pose):
    t_end = (np.pi / 2) / omega
    steps = int(round(t_end / dt))
    dt = t_end / steps                  # make the steps fit exactly
    x, y, theta = 0.0, 0.0, 0.0
    for _ in range(steps):
        x, y, theta = update(x, y, theta, v, omega, dt)
    R = v / omega
    return np.hypot(x - R, y - R)

for dt in [0.2, 0.1, 0.05, 0.01, 0.001]:
    print(f"dt = {dt:<6}  position error at a quarter lap: {quarter_lap_error(dt):.5f} m")
```

The error shrinks roughly in proportion to `dt`. That is characteristic of this simple method, called Euler integration. Halving the step halves the error. A better method uses the exact arc of a circle during each step.

```python
def update_pose_exact(x, y, theta, v, omega, dt):
    if abs(omega) < 1e-9:                                  # straight line
        return x + v * np.cos(theta) * dt, y + v * np.sin(theta) * dt, theta
    theta_new = theta + omega * dt
    x_new = x + (v / omega) * (np.sin(theta_new) - np.sin(theta))
    y_new = y - (v / omega) * (np.cos(theta_new) - np.cos(theta))
    return x_new, y_new, theta_new

for dt in [0.2, 0.05]:
    e1 = quarter_lap_error(dt)
    e2 = quarter_lap_error(dt, update=update_pose_exact)
    print(f"dt = {dt:<5} Euler error {e1:.5f} m    exact-arc error {e2:.2e} m")
```

The exact update is correct to within rounding error even with a coarse step, because it describes a circular arc exactly, which is the true motion for constant wheel speeds. The lesson is that both the model and the step size matter, although in a real robot wheel slip will dominate such small numerical effects.

## 5. In-Class Exercise

Given joint angles for a 2-link arm, compute the end-effector position by hand, then verify it with `forward_kinematics_2link`. Separately, simulate 10 steps of differential-drive motion with given wheel velocities and plot the path.

Part one. Take `theta1 = 60`, `theta2 = -30` degrees, `l1 = 0.5`, `l2 = 0.4`. Compute by hand, then check.

```python
x, y = forward_kinematics_2link(np.deg2rad(60), np.deg2rad(-30), 0.5, 0.4)
print(round(x, 4), round(y, 4))
```

The hand calculation is: elbow at `(0.25, 0.4330)`, and the second link points at 30 degrees, adding `(0.3464, 0.2)`. The tip is at `(0.5964, 0.6330)`.

Part two. Ten steps of motion with `v_l = 0.3`, `v_r = 0.5`, `L = 0.4`, `dt = 0.5`.

```python
v, omega = wheels_to_body(0.3, 0.5, 0.4)
x, y, theta = 0.0, 0.0, 0.0
path = [(x, y)]
for _ in range(10):
    x, y, theta = update_pose(x, y, theta, v, omega, 0.5)
    path.append((x, y))

path = np.array(path)
print("v =", v, "omega =", omega)
print("final pose:", round(x, 3), round(y, 3), round(np.rad2deg(theta), 1), "deg")
plt.plot(path[:, 0], path[:, 1], marker="o", color="black")
plt.axis("equal")
plt.title("Ten steps of differential-drive motion")
plt.show()
```

Questions:

1. How far, in total, has the robot turned after the ten steps? Does this equal `omega` times the elapsed time?
2. What would the path look like if `v_l` and `v_r` were swapped?
3. Which wheel speeds make the robot rotate on the spot? Try them in the simulation.

## 6. Common Mistakes

1. Using degrees where radians are needed.
2. Measuring `theta2` from the horizontal instead of from the first link, or the reverse, without noticing. State the convention.
3. Confusing the wheel separation `L` with the wheel radius.
4. Using a large time step and not noticing the integration error.
5. Forgetting that the pose update uses the heading before the turn, and that the order of updates matters.
6. Assuming the model matches the real robot. Wheel slip, uneven wheel diameters and a poorly measured `L` all produce errors, which is the reason for the odometry drift studied in Week 8.

## 7. Summary

Degrees of freedom count the independent ways a mechanism can move. Forward kinematics turns joint values into end-effector pose, and it is a chain of the transforms from Week 2. A differential-drive robot has a forward speed and a turning rate determined by its two wheel speeds, and its pose can be integrated forward in time. The accuracy depends on the time step and on the model. Next week we turn the arm problem around, and ask for the joint angles that reach a given point.

## 8. Practice Problems

1. Extend `fk_by_transforms` to a three-link arm, and plot it for a few configurations.
2. Make the 2-link arm sweep `theta1` from 0 to 90 degrees while `theta2` stays at 45 degrees, and plot the path of the end-effector.
3. Drive the differential-drive robot in a figure of eight, by switching the wheel speeds halfway.
4. Compute and plot the position error between the Euler and the exact update over time, for a lap of the circle.

## 9. Suggested Reading

1. Lynch and Park, Modern Robotics, the chapter on forward kinematics.
2. Siegwart, Nourbakhsh and Scaramuzza, Introduction to Autonomous Mobile Robots, the section on the kinematics of wheeled mobile robots.

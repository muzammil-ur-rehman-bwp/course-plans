# Week 8: Sensors I, Encoders, IMU and Odometry

## Learning Objectives

By the end of this lecture, you should be able to:

1. Convert encoder ticks into wheel distance, and wheel distances into a robot pose update.
2. Describe what an accelerometer and a gyroscope measure, and why integrating them drifts.
3. Explain why odometry errors accumulate, and distinguish systematic from random errors.
4. Simulate noisy encoders, estimate a trajectory from them, and measure the drift.
5. Review the material of Weeks 1 to 7 in preparation for the midterm.

## 1. Proprioceptive Sensors

Sensors fall into two broad groups. Exteroceptive sensors observe the outside world: cameras, LiDAR and ultrasonic sensors, which we meet from Week 9. Proprioceptive sensors observe the robot itself, such as how far the wheels have turned, how fast the body rotates, and how the joints are bent. This week is about proprioception. Together, encoders and an inertial unit give a robot a sense of its own motion, which is the basis for estimating where it is.

## 2. Wheel Encoders and Odometry

An encoder is attached to a motor or wheel shaft and produces a pulse, called a tick, for each small step of rotation. If the encoder gives `ticks_per_rev` ticks per revolution, a count of `ticks` means `ticks / ticks_per_rev` revolutions, and the distance rolled is that number times the wheel circumference.

```python
import math

def ticks_to_distance(ticks, ticks_per_rev, wheel_radius):
    revolutions = ticks / ticks_per_rev
    return revolutions * 2 * math.pi * wheel_radius

# a wheel of radius 5 cm with a 360 tick encoder
print(ticks_to_distance(360, 360, 0.05))     # one revolution: 0.3142 m
print(ticks_to_distance(90, 360, 0.05))      # a quarter turn: 0.0785 m
print(ticks_to_distance(-90, 360, 0.05))     # negative ticks mean backwards
```

The original lecture code used the value 3.14159 for pi. It is better to use `math.pi`. The distance covered by one tick is the resolution of the sensor, `2 pi r / ticks_per_rev`, which is 0.87 mm here. No distance smaller than this can be measured, which is the first source of error.

Real encoders usually have two output channels, offset by a quarter of a cycle, which lets the electronics tell the direction of rotation, and four counts per cycle. When you read a data sheet, check whether the figure given is cycles or counts per revolution, since the two differ by a factor of four.

### 2.1 From wheel distances to a pose update

Over a short interval, the left and right wheels roll distances `d_l` and `d_r`. The distance moved by the centre of the robot and the change of heading follow from the differential-drive model of Week 3:

```
d     = (d_r + d_l) / 2
d_theta = (d_r - d_l) / L
```

where `L` is the wheel separation. These are the distance versions of the velocity formulas, since distance is velocity multiplied by time. The pose is then updated, preferably with the heading at the middle of the step, which is more accurate than using the heading at the start.

```python
import numpy as np

def odometry_step(pose, d_l, d_r, L):
    x, y, theta = pose
    d = (d_r + d_l) / 2.0
    d_theta = (d_r - d_l) / L
    x += d * np.cos(theta + d_theta / 2.0)
    y += d * np.sin(theta + d_theta / 2.0)
    theta += d_theta
    return (x, y, theta)

# a robot on a gentle left curve: the left wheel rolls less than the right wheel
pose = (0.0, 0.0, 0.0)
L = 0.3
for _ in range(100):
    pose = odometry_step(pose, d_l=0.005, d_r=0.008, L=L)
print("pose after 100 steps: x = %.3f, y = %.3f, heading = %.1f deg" % (pose[0], pose[1], np.degrees(pose[2])))
```

This is the odometry computation that a ROS 2 differential-drive controller performs and publishes on `/odom`. Hand-written, it is only a few lines.

## 3. The IMU

An inertial measurement unit (IMU) typically contains two sensors, and often a third.

1. The accelerometer measures linear acceleration, along each axis, including gravity. A robot that stands still on a table still reads an upward acceleration of 9.81 m/s squared on its vertical axis. This is useful, since it tells you which way down is, and therefore the tilt of the robot.
2. The gyroscope measures angular velocity, the rate of rotation, about each axis.
3. A magnetometer, where fitted, measures the magnetic field, which gives a compass heading, but is easily disturbed by motors and metal.

Integrating the gyroscope reading over time gives the change of orientation. Integrating the accelerometer reading twice would give the change of position, but this is much less reliable, as we show next.

### 3.1 Integrating a gyroscope

Every real gyroscope has a small constant offset called a bias, and random noise. We integrate the heading of a robot that is not turning at all.

```python
rng = np.random.default_rng(0)
dt = 0.01                   # 100 Hz
steps = 6000                # 60 seconds
bias = np.deg2rad(0.1)      # 0.1 degrees per second of bias
noise_std = np.deg2rad(0.5) # noise on each reading

heading = 0.0
log = []
for k in range(steps):
    gyro = 0.0 + bias + rng.normal(0, noise_std)    # the robot is not turning
    heading += gyro * dt
    log.append(heading)

for t in [10, 30, 60]:
    print(f"after {t:2d} s: heading error {np.degrees(log[int(t / dt) - 1]):6.2f} deg")
```

The heading error grows steadily. The bias contributes an error that grows linearly with time, 0.1 degrees every second, so about 6 degrees after a minute. The noise contributes a smaller error that grows like the square root of time. Short bursts of gyroscope data are very accurate, and long integrations are not. This is typical for inertial sensors, and it is why they are corrected by other information.

### 3.2 Why double integration is worse

Position needs two integrations, so a constant acceleration error `e` produces a velocity error `e t` and a position error `e t^2 / 2`. The error grows with the square of time.

```python
rng = np.random.default_rng(1)
dt = 0.01
accel_bias = 0.02        # m/s^2, a small offset, 0.2 percent of gravity
noise = 0.05             # m/s^2 noise on each reading

v = x = 0.0
for k in range(1, 3001):                          # 30 seconds standing still
    a = accel_bias + rng.normal(0, noise)
    v += a * dt
    x += v * dt
    if k in (500, 1000, 3000):
        print(f"after {k * dt:4.0f} s standing still: estimated position error {x:7.3f} m")
```

A robot that has not moved at all believes that it has travelled about 8.5 metres after 30 seconds, from a bias of only 0.02 m/s squared. This is why an IMU is never used alone for position. In practice the bias is also not known exactly, and the orientation error would put some of gravity into the horizontal axes, which makes matters worse. It is also why the gyroscope, which needs only one integration, is much more useful than the accelerometer for motion estimation.

## 4. Odometry Drift

Odometry integrates many small measurements. Each carries a small error, and the errors do not cancel, they accumulate. The robot's believed position drifts away from its true position, and the longer it runs without an external correction, such as seeing a landmark or matching a laser scan to a map, the worse it gets.

The errors come in two kinds.

1. Systematic errors are the same every time. The wheels are not the same size, the wheel separation is not what the software believes, or the wheels are not aligned. They can be reduced by calibration.
2. Random errors differ each time. Wheel slip on a patch of dust, uneven floors and the limited resolution of the encoders are examples. They cannot be removed, only averaged.

The effect of an error depends on its type. A heading error is especially damaging, since the robot then travels in a slightly wrong direction for all the distance that follows, so a small angular error turns into a large position error far away.

### 4.1 A first illustration

The following illustrative code adds a small random error at each step, with a straight line of motion.

```python
rng = np.random.default_rng(3)

def drift_run(n_steps=100, step=0.1, noise=0.01):
    x_true = x_est = 0.0
    for _ in range(n_steps):
        x_true += step
        x_est += step + rng.normal(0, noise)          # small per-step noise
    return x_est - x_true

errors = np.array([drift_run() for _ in range(2000)])
print(f"typical error (std) after 100 steps: {errors.std():.4f} m")
print(f"expected from theory (noise * sqrt(n)): {0.01 * np.sqrt(100):.4f} m")
print(f"average error: {errors.mean():.4f} m   (close to zero: random errors cancel on average, but not in any one run)")
```

The spread of the error grows as the square root of the number of steps. The mean is near zero, but the robot only travels once, and for one run the error can be about 0.1 metres, ten times the noise of a single step.

## 5. Worked Example: Odometry from Encoder Ticks

This example is the core of the in-class exercise. We drive a robot along a square, with a known true path. We then produce simulated encoder ticks, and give the encoder model two imperfections: the right wheel is 1 percent larger than the software believes (a systematic error), and each interval has some random slip. We then compute the odometry estimate and compare it with the truth.

```python
import math
import numpy as np
import matplotlib.pyplot as plt

TICKS_PER_REV = 360
R_WHEEL = 0.05
L_BASE = 0.30
CIRC = 2 * math.pi * R_WHEEL

def true_motion_square(side=2.0, step=0.02):
    """List of (d_left, d_right) wheel distances for a square path, driven with in-place 90 degree turns."""
    moves = []
    straight_steps = int(side / step)
    # an in-place turn of 90 degrees: wheels roll opposite ways by (L/2)*(pi/2) each
    turn_total = (L_BASE / 2) * (math.pi / 2)
    turn_steps = 50
    for _ in range(4):
        moves += [(step, step)] * straight_steps
        moves += [(-turn_total / turn_steps, turn_total / turn_steps)] * turn_steps
    return moves

def encoder_ticks(moves, right_scale=1.01, slip=0.002, seed=0):
    """Convert true wheel distances into integer tick counts, with a wheel-size error and slip."""
    rng = np.random.default_rng(seed)
    ticks = []
    carry_l = carry_r = 0.0                           # fractional ticks are kept, as a real counter does
    for dl, dr in moves:
        dl_real = dl * (1 + rng.normal(0, slip))
        dr_real = dr * right_scale * (1 + rng.normal(0, slip))
        tl = dl_real / CIRC * TICKS_PER_REV + carry_l
        tr = dr_real / CIRC * TICKS_PER_REV + carry_r
        nl, nr = int(round(tl)), int(round(tr))
        carry_l, carry_r = tl - nl, tr - nr
        ticks.append((nl, nr))
    return ticks

def odometry_from_ticks(ticks):
    pose = (0.0, 0.0, 0.0)
    path = [pose]
    for tl, tr in ticks:
        d_l = tl / TICKS_PER_REV * CIRC               # same as ticks_to_distance
        d_r = tr / TICKS_PER_REV * CIRC
        pose = odometry_step(pose, d_l, d_r, L_BASE)
        path.append(pose)
    return np.array(path)

moves = true_motion_square()
true_path = [(0.0, 0.0, 0.0)]
for dl, dr in moves:
    true_path.append(odometry_step(true_path[-1], dl, dr, L_BASE))
true_path = np.array(true_path)

ticks = encoder_ticks(moves)
est_path = odometry_from_ticks(ticks)

print("true end pose:      x=%.3f y=%.3f heading=%.1f deg" % (true_path[-1, 0], true_path[-1, 1], np.degrees(true_path[-1, 2]) % 360))
print("estimated end pose: x=%.3f y=%.3f heading=%.1f deg" % (est_path[-1, 0], est_path[-1, 1], np.degrees(est_path[-1, 2]) % 360))
print("final position error: %.3f m after %.1f m of travel" % (np.hypot(*(est_path[-1, :2] - true_path[-1, :2])), 8.0))

plt.plot(true_path[:, 0], true_path[:, 1], color="black", label="true path")
plt.plot(est_path[:, 0], est_path[:, 1], color="gray", linestyle="--", label="odometry estimate")
plt.axis("equal")
plt.legend()
plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.title("Odometry drift around a 2 m square")
plt.show()
```

The true path closes, returning to the start after the fourth turn. The estimated path does not close. A 1 percent error in one wheel is enough to produce a heading error in every turn, and the heading error then makes the straight sections point the wrong way. By the end the estimate is off by about 40 centimetres, with a heading error of 17 degrees, after only 8 metres of travel.

Where is the drift worst? We can see it by comparing the position error along the way, and by trying the two kinds of motion separately.

```python
err = np.hypot(est_path[:, 0] - true_path[:, 0], est_path[:, 1] - true_path[:, 1])
for frac in [0.25, 0.5, 0.75, 1.0]:
    i = int(frac * (len(err) - 1))
    print(f"{frac * 100:3.0f}% of the way: position error {err[i]:.3f} m")

# straight line only, same imperfections
straight = [(0.02, 0.02)] * 400
t_est = odometry_from_ticks(encoder_ticks(straight))
print("straight 8 m: end at (%.3f, %.3f), heading error %.2f deg" % (t_est[-1, 0], t_est[-1, 1], np.degrees(t_est[-1, 2])))

# turns only: 8 turns in place
spin = [(-0.0047124, 0.0047124)] * 400
s_est = odometry_from_ticks(encoder_ticks(spin))
print("spin in place for 8 quarter turns: heading error %.1f deg" % (np.degrees(s_est[-1, 2]) - 8 * 90))
```

In this test the straight line produces a heading error of about 15 degrees over 8 metres, far more than the 3.7 degrees from eight quarter turns in place. Each of these has a different character. On a straight line, the unequal wheels make the robot curve, and so a systematic heading error accumulates in proportion to distance. During a turn in place, the wheel size difference changes the angle turned, so the heading error accumulates in proportion to the angle turned. This is why sharp turns and long straight lines both cause drift, and why odometry is least reliable after many turns. Once the heading is wrong, all the later distance is projected in the wrong direction.

### 5.1 Calibrating a systematic error

A systematic error can be measured and corrected. A common test is to drive the robot along a straight line for a known distance, measure the heading and distance at the end, and compute a correction factor. In our example the right wheel was 1 percent larger, so scaling its distance by 1/1.01 would remove the effect.

```python
def odometry_from_ticks_calibrated(ticks, right_correction):
    pose = (0.0, 0.0, 0.0)
    path = [pose]
    for tl, tr in ticks:
        d_l = tl / TICKS_PER_REV * CIRC
        d_r = tr / TICKS_PER_REV * CIRC * right_correction
        pose = odometry_step(pose, d_l, d_r, L_BASE)
        path.append(pose)
    return np.array(path)

cal_path = odometry_from_ticks_calibrated(ticks, right_correction=1 / 1.01)
print("uncorrected end error: %.3f m" % np.hypot(*(est_path[-1, :2] - true_path[-1, :2])))
print("calibrated end error:  %.3f m" % np.hypot(*(cal_path[-1, :2] - true_path[-1, :2])))
```

The calibrated error is much smaller. What remains is the random part, the slip and the encoder resolution, which calibration cannot remove, and which only an external correction can bound. We study that in Week 12.

## 6. Midterm Review

The midterm covers Weeks 1 to 7. Make sure you can do the following.

1. Frames and transforms. Build a rotation matrix and a homogeneous transform. Compose transforms in the right order. Convert a sensor measurement into world coordinates.
2. Kinematics. Compute forward kinematics of a 2-link arm. Solve the inverse kinematics with the law of cosines, and say when there is no solution. Apply the differential-drive model to update a pose.
3. ROS 2. Describe nodes, topics, services and parameters, and say which to use when. Read a publisher and a subscriber and say what they do. Use the command line tools to inspect a running system.
4. Dynamics and actuators. Compute the torque at a joint from a load and a distance. Use gear ratios. Check a motor against a requirement with a safety margin.
5. Odometry, from this lecture. Convert ticks to distance, update a pose, and explain why errors accumulate.

A good method of revision is to make one calculation question for each area, solve it by hand, and then check it by code. For instance: "A camera is mounted 0.2 m ahead of the robot centre, which is at (1, 2) with a heading of 90 degrees. It sees an object 2 m ahead. Where is the object in the world?" The answer is `(1, 4.2)`. Verify it with the functions from Week 2.

## 7. In-Class Exercise

Given a sequence of simulated encoder tick counts, compute the robot's estimated trajectory with odometry and plot it. Discuss where drift would be expected to be worst, for instance at sharp turns or on straight lines.

Use the tick sequence from the worked example, and also generate your own. As a start, make a figure of eight, which uses turns of both signs, and see whether the drift tends to cancel.

```python
def figure_eight(step=0.02):
    moves = []
    arc_steps = 150
    radius = 0.6
    for direction in (+1, -1):
        for _ in range(arc_steps):
            d_theta = direction * 2 * math.pi / arc_steps
            ds = radius * abs(d_theta)
            d_l = ds - (L_BASE / 2) * d_theta
            d_r = ds + (L_BASE / 2) * d_theta
            moves.append((d_l, d_r))
    return moves

moves8 = figure_eight()
truth = [(0.0, 0.0, 0.0)]
for dl, dr in moves8:
    truth.append(odometry_step(truth[-1], dl, dr, L_BASE))
truth = np.array(truth)

est = odometry_from_ticks(encoder_ticks(moves8, right_scale=1.01, seed=2))
print("figure of eight, end position error: %.3f m, heading error: %.2f deg" % (
    np.hypot(*(est[-1, :2] - truth[-1, :2])), np.degrees(est[-1, 2] - truth[-1, 2])))
```

The figure of eight ends only about 3 centimetres from the truth, but with a heading error of about 14 degrees. The two loops turn in opposite directions, so the position errors partly cancel while the heading error still remains.

Questions:

1. Does the sign of the systematic error change the shape of the error? Try `right_scale = 0.99`.
2. Which has the larger effect on position error: a 1 percent error in the wheel size or a 1 percent error in the wheel separation `L`? Try both.
3. Imagine a gyroscope with a bias of 0.1 degrees per second. How would you use it to reduce the heading error of the odometry? What new problem does it bring?

## 8. Common Mistakes

1. Using the wrong ticks per revolution, for instance the cycles instead of the counts.
2. Mixing up wheel radius and diameter.
3. Updating the pose with the old heading for the whole step, which makes turns less accurate.
4. Integrating an accelerometer for position over any length of time.
5. Forgetting that gravity appears in the accelerometer readings.
6. Treating odometry as ground truth.
7. Not calibrating the wheel sizes and wheel separation before blaming the algorithm.

## 9. Summary

Encoders measure wheel rotation, and with the differential-drive model they give a pose update at each step. An IMU measures acceleration and rotation rate, and its gyroscope is useful over short periods, but every integration of a noisy, biased signal drifts, and a double integration drifts much faster. Systematic errors can be calibrated away, but random errors accumulate for ever, so odometry on its own is never enough for long runs. Next week, after the midterm, we begin with range sensors, which provide the external view that corrects this drift.

## 10. Practice Problems

1. Modify `encoder_ticks` to use an encoder with only 90 ticks per revolution, and compare the drift. What does the lower resolution do?
2. Simulate a robot moving along a 10 m straight line, and estimate the error spread over 200 runs with random slip only, no systematic error. Compare with `noise * sqrt(n)`.
3. Use the gyroscope simulation to estimate the heading error if the bias were known and subtracted from the readings, and if the bias estimate were off by 10 percent.
4. Write a function that estimates the wheel separation `L` from a test in which the robot spins in place for exactly ten revolutions, using the tick counts.

## 11. Suggested Reading

1. Siegwart, Nourbakhsh and Scaramuzza, Introduction to Autonomous Mobile Robots, the chapter on mobile robot localization, section on odometry errors.
2. Borenstein and Feng, "Measurement and correction of systematic odometry errors in mobile robots", IEEE Transactions on Robotics and Automation, 1996.

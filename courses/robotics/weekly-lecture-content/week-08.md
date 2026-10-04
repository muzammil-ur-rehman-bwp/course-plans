# Week 8 — Lecture Content: Sensors I — Encoders, IMU, Odometry

## 1. Wheel Encoders & Odometry
An encoder counts wheel rotation (ticks per revolution). Converting ticks to distance:
```python
def ticks_to_distance(ticks, ticks_per_rev, wheel_radius):
    revolutions = ticks / ticks_per_rev
    return revolutions * 2 * 3.14159 * wheel_radius
```
Combined with the differential-drive model (Week 3), per-wheel distance traveled over a time
step gives `v_l, v_r`, which feed directly into the pose-update equation from Week 3.

## 2. IMU Basics
An Inertial Measurement Unit typically combines:
- **Accelerometer**: measures linear acceleration (including gravity).
- **Gyroscope**: measures angular velocity.
Integrating gyroscope readings over time estimates orientation change; integrating accelerometer
readings (twice) estimates position change — but this accumulates error quickly, which is why
IMU data is usually fused with other sensors (Week 12) rather than used alone for position.

## 3. Odometry Drift
Because odometry integrates small per-step measurements over time, small errors (wheel slip,
encoder resolution limits, IMU noise) accumulate ("drift") — the robot's believed position
diverges from its true position the longer it runs without correction from an external
reference (e.g., matching LiDAR scans to a known map, covered conceptually in Week 9/13).
```python
# Illustrative: simulate accumulating drift by adding small random noise each step
import numpy as np

true_positions = []
estimated_positions = []
x_true, x_est = 0.0, 0.0
for _ in range(100):
    step = 0.1
    x_true += step
    x_est += step + np.random.normal(0, 0.01)  # small per-step noise
    true_positions.append(x_true)
    estimated_positions.append(x_est)
```

## 4. Midterm Review
Review session covers: coordinate transforms, forward/inverse kinematics, ROS 2 (nodes/topics/
services), robot dynamics/actuators, and odometry — i.e., all of Weeks 1–7.

## 5. In-Class Exercise
Given a sequence of simulated encoder tick counts, compute the robot's estimated trajectory via
odometry and plot it; discuss where drift would be expected to be worst (e.g., sharp turns vs.
straight lines).

# Week 7 — Lecture Content: Robot Dynamics & Actuators

## 1. Mass, Inertia, Torque, Force
- **Force** (`F = m * a`): causes linear acceleration.
- **Torque** (`tau = r * F * sin(angle)`): the rotational analog of force, causes angular
  acceleration about a joint/axis.
- **Inertia**: resistance to acceleration (linear: mass; rotational: moment of inertia, which
  depends on mass distribution relative to the rotation axis).

## 2. Actuators: DC Motors & Servos
- **DC motor**: converts electrical current into continuous rotation; torque is roughly
  proportional to current, speed roughly proportional to voltage (simplified model).
- **Servo motor**: a DC motor plus a feedback control loop (often built-in) that holds a
  commanded angular position — common in low-cost robot arms.
- **PWM (Pulse-Width Modulation)**: the typical way to control motor speed/position by varying
  the duty cycle of a switched voltage signal (conceptual — no hardware required for this
  course's simulation-based labs).

## 3. Torque-Speed Tradeoff & Gearing
A gear reduction of ratio `N` multiplies output torque by `N` and divides output speed by `N`
(ignoring losses):
```
torque_out = torque_in * N
speed_out = speed_in / N
```
This is why robot arms use gearboxes: raw motor torque is usually far too low to lift a load
directly at useful speed/torque combinations.

## 4. Worked Example: Required Torque for an Arm Joint
For a horizontal arm link of length `r` holding a load of mass `m` at its end, the torque at the
shoulder joint due to gravity is:
```python
g = 9.81
torque_required = m * g * r  # N*m, worst case (arm fully horizontal)
```
Students compute this for a few `(m, r)` combinations and compare to typical small-servo torque
ratings (provided in a reference handout) to judge feasibility.

## 5. In-Class Exercise
Given a robot arm's load and link length, compute the required shoulder-joint torque; then,
given a wheeled robot's mass and desired acceleration, compute the required wheel torque (using
`F = m*a` and `tau = F * wheel_radius`).

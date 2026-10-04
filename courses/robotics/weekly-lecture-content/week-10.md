# Week 10 — Lecture Content: Feedback Control — PID

## 1. Open-Loop vs. Closed-Loop Control
- **Open-loop**: command is fixed, no feedback (e.g., "spin wheels at a fixed speed for 2
  seconds" regardless of what actually happens).
- **Closed-loop (feedback) control**: continuously measures the actual state and adjusts the
  command based on the error between desired and actual state — robust to disturbances and
  model inaccuracy, which open-loop control is not.

## 2. PID Controller Structure
```
error(t) = setpoint - measured_value(t)
output(t) = Kp*error(t) + Ki*integral(error) + Kd*derivative(error)
```
- **P (Proportional)**: reacts to the current error — larger error, larger correction.
- **I (Integral)**: accumulates past error — eliminates steady-state error that pure P leaves
  behind.
- **D (Derivative)**: reacts to the rate of change of error — dampens oscillation/overshoot.

```python
class PIDController:
    def __init__(self, kp, ki, kd, dt):
        self.kp, self.ki, self.kd, self.dt = kp, ki, kd, dt
        self.integral = 0.0
        self.prev_error = 0.0

    def compute(self, setpoint, measured_value):
        error = setpoint - measured_value
        self.integral += error * self.dt
        derivative = (error - self.prev_error) / self.dt
        self.prev_error = error
        return self.kp * error + self.ki * self.integral + self.kd * derivative
```

## 3. Tuning Heuristics
| Gain increased | Typical effect |
|---|---|
| Kp | Faster response, but more overshoot/oscillation if too high |
| Ki | Removes steady-state error, but can cause overshoot/instability if too high |
| Kd | Dampens oscillation, but amplifies noise if too high |

A common manual tuning approach: start with Kp only (increase until response is fast but not
wildly oscillating), add Kd to dampen overshoot, then add a small Ki to eliminate any remaining
steady-state offset.

## 4. Worked Example: Heading-Hold Task
Use the `PIDController` to drive a simulated robot's heading toward a target heading, with
`measured_value` = current heading (from odometry) and `setpoint` = target heading; the PID
output becomes the robot's angular velocity command (`/cmd_vel` angular.z).

## 5. In-Class Exercise
Given a response plot showing sustained oscillation around the setpoint, identify which gain is
likely too high and propose a tuning adjustment.

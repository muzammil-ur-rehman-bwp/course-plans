# Week 10: Feedback Control and PID

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain the difference between open-loop and closed-loop control with an example.
2. Write a PID controller and describe what each of the three terms does.
3. Simulate a plant under PID control and measure overshoot, rise time, settling time and steady-state error.
4. Tune a controller by hand, in a sensible order, and recognise which gain is wrong from the response plot.
5. Handle the practical issues: angle wrapping, output limits, integral windup and derivative noise.

## 1. Open-Loop versus Closed-Loop Control

Suppose you want a robot to drive forward 2 metres.

In open-loop control the command is fixed in advance and never corrected: "spin the wheels at 0.5 m/s for 4 seconds". If the floor is slippery, the battery is low, or one wheel drags, the robot ends up somewhere else, and nothing in the program notices. We saw this in the first lecture, where the open loop robot drove through the wall.

In closed-loop, or feedback, control the controller measures the actual state, compares it with the desired state, and adjusts the command to reduce the difference. The difference is called the error.

```
error(t) = setpoint - measured_value(t)
```

Feedback has a remarkable property. The controller does not need an accurate model of the robot. It only needs to know the direction in which the error will go down when it changes the command. This is why feedback is robust to disturbances, wear and modelling errors, which open-loop control is not.

## 2. The PID Controller

The most widely used feedback controller is the PID controller, which forms its output from three terms of the error.

```
output(t) = Kp * error(t) + Ki * integral(error) + Kd * derivative(error)
```

1. P, proportional. It reacts to the present error. The bigger the error, the bigger the correction. On its own, it usually leaves a steady-state error, a small error that does not go away, because as the error shrinks, so does the effort to remove it.
2. I, integral. It accumulates the error over time. As long as any error remains, the integral keeps growing, and so does the output, until the error is removed. It eliminates steady-state error, but adds lag, and too much of it causes overshoot and oscillation.
3. D, derivative. It reacts to how fast the error is changing, and acts like a brake, damping oscillation and overshoot. It also amplifies noise, since noise changes rapidly.

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

The controller needs the time step `dt`, since the integral and derivative are defined with respect to time. If `dt` in the controller differs from the actual interval between calls, the gains are effectively changed. In ROS 2, a timer at a fixed rate, such as 20 Hz, gives `dt = 0.05`.

### 2.1 First experiment: speed control and steady-state error

The simplest plant is a motor whose speed approaches a value proportional to the command, with some lag. We use `tau * d(speed)/dt = -speed + K * command`, so that a command of 1 gives a final speed of `K`, reached with time constant `tau`. We want the speed to reach 1.0.

```python
import numpy as np

def run_speed(kp, ki, kd, T=8.0, dt=0.01, setpoint=1.0, tau=0.5, K=1.0):
    pid = PIDController(kp, ki, kd, dt)
    speed = 0.0
    times, speeds = [], []
    for k in range(int(T / dt)):
        u = pid.compute(setpoint, speed)
        speed += dt * (-speed + K * u) / tau
        times.append(k * dt)
        speeds.append(speed)
    return np.array(times), np.array(speeds)

print("P only: the steady-state error does not vanish")
for kp in [0.5, 2.0, 10.0]:
    t, y = run_speed(kp, 0.0, 0.0)
    print(f"  Kp = {kp:5.1f}   final speed {y[-1]:.3f}   error {1.0 - y[-1]:.3f}")

t, y = run_speed(2.0, 1.5, 0.0)
print(f"PI (Kp 2, Ki 1.5): final speed {y[-1]:.3f}   error {1.0 - y[-1]:.4f}")
```

With proportional control only, the final error is `1 / (1 + K Kp)`, so it is 0.67 for `Kp = 0.5`, 0.33 for `Kp = 2`, and 0.09 for `Kp = 10`. A higher gain reduces it but never removes it. Adding an integral term drives the error towards zero. That is the standard role of the I term.

## 3. Tuning Heuristics

How the response changes as a gain is increased, with the others held fixed, is summarised below.

1. Increasing Kp gives a faster response and a smaller steady-state error, but more overshoot and oscillation, and finally instability if it is too high.
2. Increasing Ki removes steady-state error, but increases overshoot and can cause instability if it is too high.
3. Increasing Kd damps oscillation and reduces overshoot, but amplifies measurement noise if it is too high.

A common manual procedure is:

1. Set Ki and Kd to zero. Increase Kp until the response is fast but only moderately oscillatory.
2. Add Kd to damp the overshoot.
3. Add a small Ki to remove the remaining steady-state offset, and increase it only until the offset vanishes in an acceptable time.

## 4. Worked Example: Heading Hold

A standard task is to turn the robot to a target heading, and hold it. The measured value is the current heading, from odometry or an IMU, the setpoint is the target heading, and the output is the angular velocity command, which goes into `angular.z` of `/cmd_vel`.

For the simulation we need a plant. The robot's actual turning rate follows the command with a lag, because the motors cannot change speed instantly, and there is a transmission delay of 0.1 seconds before a command takes effect. The heading is the integral of the turning rate. These are real features of a robot, and they make the problem more interesting than a simple first-order system.

Two practical points have to be dealt with first. Angles wrap around, so the error must be wrapped into the interval from minus pi to pi, as in Week 2, otherwise a robot at 350 degrees asked to go to 10 degrees would turn 340 degrees the long way. And the PID controller needs to accept an error function. We extend the class to do this.

```python
from collections import deque

def wrap(a):
    return (a + np.pi) % (2 * np.pi) - np.pi

class PIDController:
    def __init__(self, kp, ki, kd, dt):
        self.kp, self.ki, self.kd, self.dt = kp, ki, kd, dt
        self.integral = 0.0
        self.prev_error = 0.0

    def compute(self, setpoint, measured_value, error_fn=None):
        error = setpoint - measured_value if error_fn is None else error_fn(setpoint, measured_value)
        self.integral += error * self.dt
        derivative = (error - self.prev_error) / self.dt
        self.prev_error = error
        return self.kp * error + self.ki * self.integral + self.kd * derivative

def run_heading(kp, ki, kd, target_deg=90.0, T=15.0, dt=0.02, tau=0.3, delay=0.1,
                disturbance=0.0, limit=100.0, noise_deg=0.0, seed=0):
    rng = np.random.default_rng(seed)
    pid = PIDController(kp, ki, kd, dt)
    target = np.deg2rad(target_deg)
    theta, omega = 0.0, 0.0
    pipeline = deque([0.0] * int(round(delay / dt)))          # commands waiting to take effect
    times, heading = [], []
    for k in range(int(T / dt)):
        measured = theta + rng.normal(0, np.deg2rad(noise_deg))
        u = pid.compute(target, measured, error_fn=lambda s, m: wrap(s - m))
        u = float(np.clip(u, -limit, limit))                   # the motors have a limit
        pipeline.append(u)
        omega += dt * (pipeline.popleft() - omega) / tau       # lag in the motor response
        theta += dt * (omega + disturbance)                    # heading is the integral of turn rate
        times.append(k * dt)
        heading.append(np.rad2deg(theta))
    return np.array(times), np.array(heading)

def describe(t, y, target=90.0):
    overshoot = max(0.0, y.max() - target) / target * 100
    rise = next((tt for tt, v in zip(t, y) if v >= 0.9 * target), float("nan"))
    outside = np.where(abs(y - target) > 2.0)[0]
    settle = 0.0 if len(outside) == 0 else (t[outside[-1] + 1] if outside[-1] + 1 < len(t) else float("nan"))
    return overshoot, rise, settle, target - y[-1]

print(f"{'Kp':>5} {'Ki':>5} {'Kd':>5}   overshoot%  rise(s)  settle(s)  final error(deg)")
for kp, ki, kd in [(0.5, 0, 0), (1.5, 0, 0), (3, 0, 0), (6, 0, 0), (10, 0, 0), (6, 0, 0.5), (6, 0, 1.0)]:
    t, y = run_heading(kp, ki, kd)
    o, r, s, e = describe(t, y)
    print(f"{kp:5.1f} {ki:5.1f} {kd:5.1f}   {o:9.1f}  {r:7.2f}  {s:9.2f}  {e:12.2f}")
```

Read the table from the top down. Low gain, `Kp = 0.5`, is slow, taking about 4 seconds to get to within 90 percent, but it does not overshoot. As `Kp` rises the response is faster, but it overshoots more. At `Kp = 10` the table shows a settling time of `nan` and a final error of many degrees, which means that the heading never settles. The delay in the loop makes high gain unstable, and the robot swings back and forth for ever. This is the situation of the in-class question below.

The last two rows show the remedy. With `Kp = 6` and a derivative gain of 1.0, the overshoot is a third of what it was without D, and the response settles in about 1.2 seconds instead of 5.

Plot the contrasts.

```python
import matplotlib.pyplot as plt

for kp, ki, kd, style in [(1.5, 0, 0, ":"), (6, 0, 0, "--"), (6, 0, 1.0, "-")]:
    t, y = run_heading(kp, ki, kd)
    plt.plot(t, y, style, color="black", label=f"Kp={kp}, Kd={kd}")
plt.axhline(90, color="gray", linewidth=0.8)
plt.xlim(0, 6)
plt.xlabel("time (s)")
plt.ylabel("heading (deg)")
plt.title("Heading hold with three gain settings")
plt.legend()
plt.show()
```

### 4.1 A constant disturbance and the integral term

Real robots do not drive straight. Suppose a mismatch of the wheels adds an unwanted turn of 0.05 radians per second, which is about 3 degrees per second. With P and D alone, the controller settles with a permanent error, because a nonzero error is needed to produce the command that cancels the disturbance.

```python
print(f"{'Kp':>4} {'Ki':>4} {'Kd':>4}   peak(deg)  final error(deg)")
for kp, ki, kd in [(3, 0, 0.5), (3, 0.3, 0.5), (3, 1.0, 0.5), (3, 3.0, 0.5)]:
    t, y = run_heading(kp, ki, kd, disturbance=0.05)
    print(f"{kp:4.1f} {ki:4.1f} {kd:4.1f}   {y.max():8.1f}  {90 - y[-1]:12.2f}")
```

With `Ki = 0` the robot sits almost 1 degree away from the target, forever. With a larger `Ki` that error vanishes, but the price is overshoot, since the integral has built up while the robot was approaching the target, and has to be unwound afterwards. Too large an integral gain, `Ki = 3`, gives an overshoot of almost 40 percent. Neither extreme is good, and a moderate value, `Ki = 1`, removes the offset within a few seconds with moderate overshoot.

### 4.2 Practical details

1. Output limits. Real actuators saturate. The controller above clips its output to the range of the motors. When the output is clipped while a large error persists, the integral term continues to grow, which is called windup, and the controller then overshoots badly when the error finally changes sign. A simple anti-windup measure is to stop integrating while the output is saturated, or to limit the integral itself.
2. Derivative noise. The derivative term amplifies noise. In practice we filter the measurement, or the derivative, and use a modest Kd.
3. Derivative kick. If the setpoint changes suddenly, the error jumps, and the derivative of a jump is a huge spike. A common remedy is to take the derivative of the measurement instead of the error.
4. Timing. The controller assumes a regular `dt`. A late or jittery control loop changes the effective gains.

We now see the effect of noise on Kd, and then add a basic anti-windup.

```python
print("derivative gain with 0.5 degree sensor noise: spread of the held heading (deg)")
for kd in [0.0, 0.5, 2.0, 5.0]:
    t, y = run_heading(3, 0, kd, noise_deg=0.5)
    print(f"  Kd = {kd:3.1f}   standard deviation of the last 6 s: {y[-300:].std():.3f}")
```

A moderate `Kd` helps. A large one, `Kd = 5`, makes the loop go wild, since each noisy sample is turned into a large command. This is the reason for the warning in the tuning table.

```python
class PIDWithAntiWindup(PIDController):
    def __init__(self, kp, ki, kd, dt, out_limit):
        super().__init__(kp, ki, kd, dt)
        self.out_limit = out_limit

    def compute(self, setpoint, measured_value, error_fn=None):
        error = setpoint - measured_value if error_fn is None else error_fn(setpoint, measured_value)
        derivative = (error - self.prev_error) / self.dt
        self.prev_error = error
        unsat = self.kp * error + self.ki * self.integral + self.kd * derivative
        out = float(np.clip(unsat, -self.out_limit, self.out_limit))
        if out == unsat or (error * unsat) < 0:        # integrate only if not pushing further into saturation
            self.integral += error * self.dt
        return out

def run_limited(controller_cls, limit=1.0, T=15.0, dt=0.02, target_deg=150.0):
    kwargs = {"out_limit": limit} if controller_cls is PIDWithAntiWindup else {}
    pid = controller_cls(2.0, 1.0, 0.5, dt, **kwargs)
    target = np.deg2rad(target_deg)
    theta, omega, peak = 0.0, 0.0, 0.0
    for k in range(int(T / dt)):
        u = pid.compute(target, theta, error_fn=lambda s, m: wrap(s - m))
        u = float(np.clip(u, -limit, limit))
        omega += dt * (u - omega) / 0.3
        theta += dt * omega
        peak = max(peak, np.rad2deg(theta))
    return peak

print("a large 150 degree turn with the turn rate limited to 1 rad/s")
print("  plain PID peak heading:      %.1f deg" % run_limited(PIDController))
print("  with anti-windup peak heading: %.1f deg" % run_limited(PIDWithAntiWindup))
```

The plain controller swings out to about 234 degrees, 84 degrees past the target, since the integral wound up while the robot was saturated and unable to turn faster. The anti-windup version peaks at about 158 degrees. This matters in real robots, which are always limited by their motors.

## 5. In-Class Exercise

Given a response plot showing sustained oscillation around the setpoint, identify which gain is likely too high and propose a tuning adjustment.

Create such a plot yourself. Use `run_heading(10, 0, 0)` and look at the trace. The heading never settles, but swings back and forth around 90 degrees, with an amplitude that does not decay.

```python
t, y = run_heading(10, 0, 0)
plt.plot(t, y, color="black")
plt.axhline(90, color="gray", linewidth=0.8)
plt.xlabel("time (s)")
plt.ylabel("heading (deg)")
plt.title("Sustained oscillation: Kp is too high")
plt.show()

peaks = [i for i in range(1, len(y) - 1) if y[i] > y[i - 1] and y[i] >= y[i + 1]]
print("oscillation period (s):", round(float(np.mean(np.diff(t[peaks]))), 2))
print("heading swings between %.0f and %.0f degrees during the last 5 s" % (y[-250:].min(), y[-250:].max()))
```

A good answer says that the proportional gain is too high for the delay and lag in the plant, since the correction arrives too late and pushes the heading past the target again. The remedy is to lower `Kp`, perhaps to about 4, and to add a derivative term to provide damping. Check by running the simulation. Lowering `Kp` to 4 stops the endless swinging but still overshoots by 41 percent. Adding `Kd = 0.7` cuts the overshoot to about 10 percent and the settling time to 1.3 seconds.

```python
for kp, kd in [(10, 0), (4, 0), (4, 0.7)]:
    t, y = run_heading(kp, 0, kd)
    o, r, s, e = describe(t, y)
    print(f"Kp={kp:4.1f} Kd={kd:3.1f}: overshoot {o:5.1f}%  settle {s:5.2f} s  final error {e:6.2f} deg")
```

Questions:

1. In the oscillating plot, what is the approximate period of the swing? What gain would you raise to make it larger, and why?
2. If a plot shows a slow creep towards the setpoint that has not arrived after a long time, which gain is too low?
3. If the response is quick and has the right final value, but the output command is very jittery, which gain would you reduce?

## 6. Common Mistakes

1. Using a raw angle difference without wrapping.
2. Calling the controller at an irregular rate, or with a wrong `dt`.
3. Tuning all three gains at once. Tune in order, P then D then I.
4. Forgetting output limits, and the windup that follows.
5. A very high Kd on a noisy measurement.
6. Taking a plot with a small steady-state error and increasing Kp indefinitely, instead of adding some integral.
7. Using the integral term when it is not needed. A plain P or PD controller is simpler and may be good enough.

## 7. Summary

Feedback control corrects errors that no open-loop plan can foresee. A PID controller combines the present error, its accumulation and its rate of change. P gives the basic response, I removes steady-state error, and D adds damping. Delay and lag in the plant limit how large the gains can be, and practical issues of angle wrapping, saturation, windup and noise decide whether a controller that works on paper also works on a robot. Next week we turn to vision, a rich sensor that the controller can use.

## 8. Practice Problems

1. Using `run_speed`, find the smallest `Ki` for `Kp = 2` that brings the speed to within 1 percent of the setpoint within 3 seconds.
2. Add a first-order low-pass filter to the derivative term of `PIDController`, and repeat the noise experiment with `Kd = 5`.
3. Implement a PID that follows a changing target heading, a slow ramp of 10 degrees per second, and measure the lag error. Which term would remove a ramp-following error?
4. Write a ROS 2 style node, using the test bench of Week 6, that subscribes to `/odom`, computes the heading error to a target, and publishes a `Twist` on `/cmd_vel`.

## 9. Suggested Reading

1. Astrom and Murray, Feedback Systems: An Introduction for Scientists and Engineers, free online, the chapter on PID control.
2. Siegwart, Nourbakhsh and Scaramuzza, Introduction to Autonomous Mobile Robots, the section on control.

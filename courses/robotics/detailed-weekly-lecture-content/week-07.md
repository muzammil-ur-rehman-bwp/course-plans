# Week 7: Robot Dynamics and Actuators

## Learning Objectives

By the end of this lecture, you should be able to:

1. Use force, torque and inertia to reason about how a robot accelerates.
2. Describe how a DC motor and a servo motor work, using a simplified model.
3. Explain PWM and how it controls the average voltage at a motor.
4. Use gear ratios to trade speed for torque.
5. Compute the torque needed at an arm joint and at the wheels of a mobile robot, and judge whether a given motor is adequate.

## 1. Why Dynamics?

Kinematics, in Weeks 3 and 4, describes where a robot goes. Dynamics asks what it takes to get it there. A robot arm can have perfectly correct inverse kinematics and still fail, because its motors are too weak to hold the arm up. A mobile robot can follow the correct velocity commands in simulation and, on real hardware, accelerate far too slowly because its motors are matched to a lighter body. Before building or buying anything, a quick dynamics calculation tells you whether the idea is feasible.

We keep the physics simple. The goal is to be able to do back of the envelope estimates, with units checked, and to know which quantities matter.

## 2. Mass, Inertia, Force and Torque

1. Force, `F = m a`, causes linear acceleration. The unit is the newton. A force of 1 N gives a mass of 1 kg an acceleration of 1 m/s squared.
2. Torque, `tau = r F sin(angle)`, is the rotational analogue of force. It is the turning effect of a force applied at a distance `r` from an axis, and it causes angular acceleration. The unit is the newton metre. The angle is that between the force and the lever arm, and the torque is greatest when the force is at right angles to it.
3. Inertia is resistance to a change of motion. For linear motion it is the mass. For rotation it is the moment of inertia, which depends on how the mass is distributed relative to the axis: the further the mass lies from the axis, the harder it is to spin. The rotational counterpart of Newton's law is `tau = I alpha`.

Moments of inertia of two common shapes, about the axis through the pivot, are worth remembering. A point mass `m` at distance `r` has `I = m r^2`. A thin uniform rod of mass `m` and length `L` pivoted at one end has `I = m L^2 / 3`.

```python
def inertia_point_mass(m, r):
    return round(m * r**2, 6)

def inertia_rod_about_end(m, length):
    return m * length**2 / 3.0

# the same 1 kg, placed differently, behaves differently when spun
print("1 kg at 0.1 m :", inertia_point_mass(1.0, 0.1), "kg m^2")
print("1 kg at 0.5 m :", inertia_point_mass(1.0, 0.5), "kg m^2")
print("1 kg rod, 0.5 m long, pivoted at its end:", round(inertia_rod_about_end(1.0, 0.5), 4), "kg m^2")
```

Moving the same mass five times further out multiplies the inertia by 25. This is the reason robot designers place heavy parts, such as motors, near the base of an arm and not at its tip.

## 3. Actuators: DC Motors and Servos

### 3.1 The DC motor

A DC motor converts electrical power into rotation. In a simple model there are two proportional relationships.

1. Torque is roughly proportional to current: `tau = Kt * i`.
2. The motor also generates a back voltage proportional to its speed: `V_back = Ke * omega`. For a motor in consistent SI units, `Kt` and `Ke` are numerically equal.

The supply voltage `V` has to overcome the resistance of the windings and the back voltage, so `V = R i + Ke omega`. Two extreme cases follow. At stall, when the shaft is not turning, there is no back voltage and the current and torque are at their maximum, `tau_stall = Kt V / R`. With no load, the speed is highest, with `omega_free = V / Ke`, ignoring friction. Between these, torque falls linearly as speed rises. This is the torque-speed curve of a DC motor, and it is the first thing to read on a motor's datasheet.

The following simulation uses the electrical and mechanical equations with the parameters of a small motor, to see how the speed responds to a voltage step.

```python
import numpy as np

# motor parameters (small hobby-size DC motor, SI units)
Kt = 0.02       # torque constant, N m / A  (also the back-EMF constant in V s / rad)
R = 2.0         # winding resistance, ohm
J = 1e-5        # rotor inertia, kg m^2
b = 1e-6        # viscous friction, N m s / rad
V = 12.0        # supply voltage, V

dt = 0.0005
omega = 0.0
speeds = []
for step in range(int(0.5 / dt)):
    current = (V - Kt * omega) / R            # electrical equation
    torque = Kt * current
    domega = (torque - b * omega) / J         # mechanical equation: J d(omega)/dt = torque - friction
    omega += domega * dt
    speeds.append(omega)

omega_ss = Kt * V / (R * b + Kt * Kt)
print(f"speed after 0.5 s:      {omega:7.1f} rad/s  ({omega * 60 / (2 * np.pi):7.0f} rpm)")
print(f"theoretical steady state: {omega_ss:5.1f} rad/s")
print(f"stall torque:           {Kt * V / R:.3f} N m")
t63 = next(i for i, w in enumerate(speeds) if w >= 0.632 * omega_ss) * dt
print(f"time to reach 63% of final speed (time constant): {t63 * 1000:.0f} ms")
```

The speed rises quickly and levels off at the steady state, where the back voltage balances the supply. The time constant, here a few tens of milliseconds, tells you how fast the motor responds. The motor speeds up very quickly with no load, which is exactly why gearing is needed for useful torque.

The stall torque in this example, 0.12 N m, is typical of a small motor. Remember that operating a motor near stall for long draws maximum current and overheats it.

### 3.2 The servo motor

A servo is a DC motor combined with a gearbox, a position sensor, and a small built-in control loop. You command an angle, and the servo moves there and holds it, working against the load. This is why servos are the standard choice for low-cost robot arms: the feedback control is built in. Servos are usually specified by their stall torque, in N m or kg cm, and by their speed for a 60 degree turn.

A caution on units. Hobby servo data sheets quote torque in kg cm. Multiply by 0.0981 to convert to N m, since 1 kg cm is 1 kgf times 1 cm, which is `9.81 * 0.01 = 0.0981` N m.

```python
def kgcm_to_nm(kgcm):
    return kgcm * 9.81 / 100.0

for rating in [2.0, 10.0, 20.0, 35.0]:
    print(f"{rating:5.1f} kg cm = {kgcm_to_nm(rating):.3f} N m")
```

### 3.3 PWM

Motors are rarely driven by a variable analogue voltage. It is more efficient to switch the full voltage on and off very rapidly. This is pulse width modulation (PWM). The fraction of each cycle for which the switch is on is the duty cycle, and the motor, which smooths the pulses because of its inductance and inertia, responds to the average voltage, `V_avg = duty * V_supply`.

For servos, the duty cycle (more exactly the pulse width) is read as a position command, not as a power level.

```python
def pwm_waveform(duty, freq_hz=1000, sim_time=0.01, samples_per_cycle=100, v_supply=12.0):
    n = int(sim_time * freq_hz * samples_per_cycle)
    t = np.arange(n) / (freq_hz * samples_per_cycle)
    phase = (t * freq_hz) % 1.0
    return t, np.where(phase < duty, v_supply, 0.0)

for duty in [0.25, 0.5, 0.75]:
    t, v = pwm_waveform(duty)
    print(f"duty {duty:4.2f}: average voltage {v.mean():5.2f} V  (expected {duty * 12:5.2f} V)")
```

No hardware is needed for the labs in this course, because the simulator takes velocity commands directly. The idea is worth knowing, since a real motor driver does exactly this.

## 4. Torque-Speed Tradeoff and Gearing

Motors tend to be fast and weak, and robot joints need to be slow and strong. A gearbox converts one into the other. A reduction of ratio `N` multiplies the output torque by `N` and divides the output speed by `N`, ignoring losses.

```
torque_out = torque_in * N
speed_out  = speed_in / N
```

Power, which is torque times speed, is conserved apart from losses, so you can never gain both. Real gearboxes also lose some power to friction, expressed by an efficiency, typically 60 to 90 percent for a small gearbox and much less for some worm drives.

This is why robot arms use gearboxes. The raw torque of the small motor above, 0.12 N m at stall, would never hold up a loaded arm. With a 50:1 reduction and an efficiency of 80 percent:

```python
def geared_output(torque_in, speed_in, N, efficiency=1.0):
    return torque_in * N * efficiency, speed_in / N

stall_torque = Kt * V / R
free_speed = V / Kt                                  # rad/s, ignoring friction
tau_out, w_out = geared_output(stall_torque, free_speed, N=50, efficiency=0.8)
print(f"motor alone : stall torque {stall_torque:.3f} N m, free speed {free_speed:.0f} rad/s")
print(f"after 50:1  : stall torque {tau_out:.2f} N m, free speed {w_out:.1f} rad/s ({w_out * 60 / (2 * np.pi):.0f} rpm)")
```

The output shaft turns at about 115 rpm and can deliver about 4.8 N m at stall, an increase of forty times in torque, paid for with a fiftyfold reduction in speed. Gearing also reduces the effect of the load's inertia as seen by the motor, by a factor of `N` squared, which makes the motor easier to control.

One further consequence is that a gearbox with a high ratio resists being turned backwards from the output. This is useful for holding a position without power, but it means that a high-ratio robot arm is difficult to move by hand, and collisions can damage the gears.

## 5. Worked Example: Required Torque for an Arm Joint

For a horizontal arm link of length `r` holding a load of mass `m` at its end, the torque at the shoulder joint due to gravity is the weight times the lever arm:

```
g = 9.81
torque_required = m * g * r      # N m, worst case: the arm is fully horizontal
```

This is the worst case, because the lever arm is longest when the arm is horizontal. At an angle `phi` above the horizontal the torque is `m g r cos(phi)`.

Let us extend it to include the weight of the link itself. A uniform link of mass `m_link` has its weight acting at its middle, so it adds `m_link g r / 2`. Then we compare the result with a servo rating.

```python
g = 9.81

def shoulder_torque(m_load, r, m_link=0.0, phi_deg=0.0):
    phi = np.deg2rad(phi_deg)
    return (m_load * g * r + m_link * g * r / 2.0) * np.cos(phi)

servo_nm = kgcm_to_nm(20.0)     # a 20 kg cm servo
print(f"servo stall torque: {servo_nm:.2f} N m")
print()
print(f"{'load kg':>8} {'reach m':>8} {'link kg':>8} {'torque N m':>11}   verdict")
for m_load, r, m_link in [(0.1, 0.20, 0.05), (0.3, 0.25, 0.10), (0.5, 0.30, 0.15), (1.0, 0.30, 0.20)]:
    tau = shoulder_torque(m_load, r, m_link)
    # keep a safety margin: never ask for more than half of the stall torque
    verdict = "OK" if tau < 0.5 * servo_nm else ("marginal" if tau < servo_nm else "too weak")
    print(f"{m_load:8.2f} {r:8.2f} {m_link:8.2f} {tau:11.3f}   {verdict}")
```

The first two rows are well within the capability of the servo. The third needs 1.69 N m against 1.96 N m available, so it is possible but too close to the limit. The last needs 3.24 N m, which is far beyond the servo's 1.96 N m, so it cannot do it, whatever its control looks like. Notice the safety margin in the code. A motor held near its stall torque draws maximum current and overheats, so engineers choose a motor with at least a factor of two to spare.

How does the torque change as the arm lifts?

```python
for phi in [0, 30, 60, 90]:
    print(f"arm {phi:2d} deg above horizontal: torque {shoulder_torque(0.5, 0.30, 0.15, phi):.3f} N m")
```

At 90 degrees, with the arm straight up, gravity gives no torque at all in this simple model.

## 6. Worked Example: Wheel Torque for a Mobile Robot

For the wheeled robot, the tractive force needed at the ground is `F = m a` plus any resisting forces. The torque at each of the two wheels is then the force per wheel times the wheel radius, `tau = F * wheel_radius`.

```python
def wheel_torque(mass, accel, wheel_radius, n_driven=2, rolling_coeff=0.0, slope_deg=0.0):
    g = 9.81
    f_accel = mass * accel
    f_roll = rolling_coeff * mass * g * np.cos(np.deg2rad(slope_deg))
    f_slope = mass * g * np.sin(np.deg2rad(slope_deg))
    f_total = f_accel + f_roll + f_slope
    return f_total / n_driven * wheel_radius

# a 5 kg robot, accelerating at 0.5 m/s^2, wheel radius 5 cm
print("ideal, no friction: ", round(wheel_torque(5.0, 0.5, 0.05), 4), "N m per wheel")
print("with rolling friction (0.02):", round(wheel_torque(5.0, 0.5, 0.05, rolling_coeff=0.02), 4), "N m per wheel")
print("on a 10 degree slope:        ", round(wheel_torque(5.0, 0.5, 0.05, rolling_coeff=0.02, slope_deg=10), 4), "N m per wheel")
```

The ideal figure is 0.0625 N m per wheel, which sounds tiny. With rolling friction it rises to 0.087 N m, and on a gentle slope of 10 degrees it jumps to about 0.30 N m, nearly five times the ideal. Slopes dominate. A robot that is fine on a flat floor may not climb a ramp.

The figures also give the maximum speed for a given motor, since a wheel speed `omega` gives a ground speed `v = omega * wheel_radius`.

```python
free_speed_after_gearbox = 12.0                          # rad/s, from the 50:1 example above
print("top speed of the robot: %.2f m/s" % (free_speed_after_gearbox * 0.05))
print("torque available per wheel (80%% efficient 50:1 gearbox, with some margin): %.2f N m" % (4.8 * 0.5))
```

The torque available is about 2.4 N m per wheel with a margin of two, while about 0.30 N m is needed, so a 50:1 gearbox is far more than enough here. A smaller ratio would give a higher top speed. This is the kind of design trade a real project has to make.

## 7. In-Class Exercise

Given a robot arm's load and link length, compute the required shoulder-joint torque. Then, given a wheeled robot's mass and desired acceleration, compute the required wheel torque, using `F = m a` and `tau = F * wheel_radius`.

A suggested problem set:

1. Arm: a load of 0.4 kg held at the end of a 0.35 m link whose own mass is 0.12 kg. Compute the horizontal torque, and say if a 15 kg cm servo is enough with a safety factor of two.
2. Wheeled robot: 8 kg, desired acceleration 0.8 m/s squared, wheel radius 0.06 m, two driven wheels, rolling friction coefficient 0.03.

```python
tau1 = shoulder_torque(0.4, 0.35, 0.12)
rating = kgcm_to_nm(15.0)
print(f"arm: required {tau1:.3f} N m, servo {rating:.3f} N m, margin factor {rating / tau1:.2f}")

tau2 = wheel_torque(8.0, 0.8, 0.06, rolling_coeff=0.03)
print(f"wheel: required {tau2:.3f} N m per wheel")
```

For the first problem, the required torque is about 1.58 N m against about 1.47 N m from the servo, so the servo is too weak even without any margin. In the second case the torque is about 0.26 N m per wheel.

Questions:

1. By what factor would the arm torque change if the link length were doubled and the load stayed the same?
2. Why is torque, not power, usually the limiting quantity for a robot arm at rest?
3. A 100:1 gearbox with 50 percent efficiency is fitted to the motor of section 3.1. What are the output stall torque and the free speed?

## 8. Common Mistakes

1. Mixing units: kg cm and N m, rpm and rad per second, grams and kilograms.
2. Using the best case, such as the arm angled upwards, instead of the worst case, when sizing a motor.
3. Forgetting the weight of the arm itself.
4. Choosing a motor that can only just do the job, with no safety margin for friction, slopes or acceleration.
5. Ignoring gearbox efficiency.
6. Forgetting that motor speed and torque trade off, and choosing a motor by one number only.

## 9. Summary

Force and torque, with the inertia that opposes them, describe how things accelerate. A DC motor produces torque from current and has a speed limit set by its back voltage. A servo adds sensing and feedback so that it holds an angle. PWM controls the average voltage by fast switching. Gearing trades speed for torque, and design starts with a worst-case torque calculation compared against the actuator rating, with a margin. Next week we look at how a robot measures its own motion.

## 10. Practice Problems

1. A 2-link arm has `l1 = 0.4 m`, `l2 = 0.3 m`, link masses 0.3 kg and 0.2 kg, and a 0.2 kg load at the tip. Compute the worst-case torque at the shoulder and at the elbow.
2. Plot the torque-speed line of the motor in section 3.1, and mark the operating point when a gearbox drives a load needing 0.05 N m at the motor shaft.
3. For the mobile robot, find the highest slope angle the robot can climb with a steady speed, if each wheel can deliver 0.3 N m.
4. Simulate the motor with a PWM supply of 50 percent duty at 1 kHz and compare its speed with that of a constant 6 V supply.

## 11. Suggested Reading

1. Siegwart, Nourbakhsh and Scaramuzza, Introduction to Autonomous Mobile Robots, the section on actuators and locomotion.
2. Craig, Introduction to Robotics: Mechanics and Control, the chapter on dynamics.
3. Any motor data sheet, read slowly, with the torque-speed curve in front of you.

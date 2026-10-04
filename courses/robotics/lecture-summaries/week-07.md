# Week 7 Summary — Robot Dynamics & Actuators

**Key takeaways:**
- Torque is the rotational analog of force; inertia resists angular/linear acceleration.
- DC motors convert current to rotation; servos add built-in position feedback control; PWM is
  the typical control signal.
- Gearing trades speed for torque (`torque_out = torque_in * N`, `speed_out = speed_in / N`),
  which is why robot arms need gearboxes to lift practical loads.
- Required torque for a horizontal arm link is `m * g * r` in the worst case (fully extended).

**You should now be able to:** compute required torque for a simple arm/wheel scenario; explain
the torque-speed tradeoff introduced by gearing.

**Next week:** sensors (encoders, IMU) and odometry, plus the midterm review.

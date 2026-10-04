# Lab Notes 10 — PID Controller Design & Tuning

**Concept recap:** `output = Kp*error + Ki*integral(error) + Kd*derivative(error)`; tune P first
for responsiveness, then D to dampen overshoot, then a small I to remove steady-state error.

**Common pitfalls:**
- Integral term growing unbounded ("integral windup") if the system can't actually reach the
  setpoint for a while — consider clamping the integral term if this occurs.
- Derivative term amplifying sensor noise into a jittery control signal — a small amount of
  measurement smoothing can help, but don't over-filter and introduce lag.
- Using inconsistent `dt` between control loop iterations, which breaks the integral/derivative
  calculations' assumptions.

**Debugging tip:** always plot the response curve, not just a final error number — oscillation,
overshoot, and steady-state error all look different in the final-error metric but very
different in the time-series plot.

**Instructor tip:** have students tune by hand (not an auto-tuner) at least once — the
P-then-D-then-I progression in Tasks B/C is the point, not just reaching a good final result.

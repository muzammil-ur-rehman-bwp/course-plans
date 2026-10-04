# Week 10 Summary — Feedback Control: PID

**Key takeaways:**
- Closed-loop control continuously corrects based on measured error, unlike open-loop control.
- PID combines proportional (current error), integral (accumulated error), and derivative (rate
  of change of error) terms to compute a control output.
- Each gain has a characteristic effect: Kp speeds response (risking overshoot), Ki removes
  steady-state error (risking instability if too high), Kd dampens oscillation (risking noise
  amplification if too high).
- A heading-hold task is a direct, concrete application: PID output becomes the angular velocity
  command sent to `/cmd_vel`.

**You should now be able to:** implement a PID controller; tune its gains based on observed
response behavior.

**Reminder:** Capstone project proposal due this week.
**Next week:** computer vision for robotics.

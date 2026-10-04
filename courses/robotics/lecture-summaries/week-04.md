# Week 4 Summary — Inverse Kinematics

**Key takeaways:**
- IK finds joint angles for a desired end-effector position; it can have multiple solutions
  (elbow up/down) or none (unreachable target).
- Analytical IK for a 2-link arm uses the law of cosines; reachability is checked via
  `|l1-l2| <= d <= l1+l2`.
- Numerical/iterative (Jacobian-based) IK is used when analytical solutions aren't practical
  (more joints/complex geometry) — conceptually similar to gradient-based optimization.
- Sweeping forward kinematics across all joint angles visualizes the reachable workspace.

**You should now be able to:** solve 2-link IK analytically; explain why a target might be
unreachable; visualize an arm's workspace.

**Reminder:** Assignment 1 (kinematics) is assigned this week, due start of Week 6.
**Next week:** Introduction to ROS 2 — the middleware we'll use to actually drive simulated
robots for the rest of the course.

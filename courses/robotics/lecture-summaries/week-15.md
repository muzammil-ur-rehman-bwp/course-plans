# Week 15 Summary — Autonomous Navigation Pipeline

**Key takeaways:**
- The full navigation pipeline (sense → estimate → plan → control) is an integration of
  components already built individually in Weeks 9, 10, 12, and 13/14 — no new theory, just
  wiring.
- Nav2 is the production-grade ROS 2 analog of this hand-built pipeline; studying it gives
  context for how this course's simplified version relates to real-world systems.
- A minimal pipeline subscribes to sensing/odometry topics, maintains an occupancy grid and
  planned path, and republishes a PID-computed `/cmd_vel` command toward the next waypoint.

**You should now be able to:** assemble perception, estimation, planning, and control into one
running ROS 2 navigation pipeline in simulation.

**Reminder:** Assignment 3 due at the start of this week.
**Next week:** capstone presentations, course review, and robotics ethics/safety.

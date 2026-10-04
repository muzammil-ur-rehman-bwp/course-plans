# Week 8 Summary — Sensors I: Encoders, IMU, Odometry; Midterm Review

**Key takeaways:**
- Wheel encoders convert tick counts to distance, feeding the differential-drive pose-update
  equations from Week 3.
- IMUs measure acceleration and angular velocity; integrating these over time estimates motion,
  but accumulates error quickly if used alone.
- Odometry drift grows over time because small per-step errors accumulate — this motivates
  sensor fusion (Week 12) and using external references (e.g., LiDAR/mapping) to correct it.
- Midterm (Week 9) covers all of Weeks 1–8: transforms, kinematics, ROS 2, dynamics, odometry.

**You should now be able to:** compute odometry from encoder data; explain why odometry drifts;
self-assess readiness for the midterm.

**Reminder:** Assignment 1 is due at the start of this week. Midterm Exam is next week (Week 9).

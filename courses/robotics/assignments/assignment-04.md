# Assignment 4 — Odometry, Sensor Fusion & Pipeline Design (Week 14–15 recap)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 14 | **Due:** Start of Week 16

## Instructions
Submit `assignment04.ipynb` with working code and written answers for all questions.

## Questions
1. **(Odometry, 20 pts)** Given a new encoder tick sequence (different from Lab 8's), compute
   the estimated trajectory and report the final position error against the provided ground
   truth.
2. **(Kalman filter, 25 pts)** Apply a 1D Kalman filter to fuse the Question 1 odometry estimate
   (treated as the "measurement") with a simple constant-velocity motion model; report whether
   the fused estimate reduces error compared to raw odometry alone.
3. **(Pipeline design, 25 pts)** Without necessarily implementing it fully, sketch (as a diagram
   or structured description) a navigation pipeline for a new scenario: a robot with a LiDAR and
   camera that must navigate to a visually-identified goal object while avoiding obstacles.
   Identify which course technique fills each pipeline stage.
4. **(Reflection, 30 pts)** Write 6–8 sentences reflecting on which course technique (kinematics,
   ROS 2, control, vision, estimation, or planning) you found most challenging and why, and how
   it connects to your capstone project's planned approach.

## Submission
Upload `assignment04.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

# Assignment 1 — Kinematics (Weeks 2–4)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 4 | **Due:** Start of Week 6

## Instructions
Submit `assignment01.ipynb` with working code and written answers for all questions.

## Questions
1. **(Transforms, 15 pts)** Given a robot pose `T_world_robot` and a sensor offset
   `T_robot_sensor` (both provided as specific values), compute `T_world_sensor` and transform a
   given point from the sensor frame to the world frame.
2. **(Forward kinematics, 20 pts)** For a 3-link planar arm (extend the 2-link FK equations to 3
   links), implement forward kinematics and compute the end-effector position for 3 given joint
   angle sets.
3. **(Differential drive, 20 pts)** Given a sequence of `(v_l, v_r)` wheel velocity commands and
   time steps, simulate and plot the robot's trajectory. Compare the final position to a
   straight-line approximation and discuss the difference.
4. **(Inverse kinematics, 25 pts)** For a 2-link arm with given link lengths, compute both IK
   solutions for 3 target points, and identify (with justification) one target point that is
   unreachable.
5. **(Reflection, 20 pts)** Write 4–6 sentences explaining, in your own words, why a 3-link arm's
   inverse kinematics is generally harder to solve analytically than a 2-link arm's, connecting
   this to the Week 4 discussion of numerical/iterative IK methods.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

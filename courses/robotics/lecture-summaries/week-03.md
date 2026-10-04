# Week 3 Summary — Forward Kinematics

**Key takeaways:**
- DOF counts independent configuration parameters; revolute joints use angles, prismatic joints
  use distances.
- Forward kinematics for a 2-link arm chains joint rotations/link lengths to compute end-effector
  position — a direct application of Week 2's transform composition.
- The differential-drive model converts wheel velocities to linear/angular velocity, then
  integrates over time to update pose `(x, y, theta)` — the same math underlying ROS 2 odometry.

**You should now be able to:** compute forward kinematics for a 2-link arm; simulate a
differential-drive robot's trajectory given wheel velocities.

**Next week:** inverse kinematics — going from a desired end-effector position back to the
required joint angles.

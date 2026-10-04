# Week 2 Summary — Math Foundations: Frames & Transformations

**Key takeaways:**
- Robots work across multiple coordinate frames (world, robot, sensor); converting between them
  is foundational to kinematics, odometry, and perception.
- A rotation matrix encodes orientation; a homogeneous transform combines rotation + translation
  into one matrix, enabling composition via matrix multiplication.
- Chaining transforms (`T_world_robot @ T_robot_sensor`) converts a sensor-frame measurement to
  world coordinates — a pattern reused when processing LiDAR/camera data later in the course.

**You should now be able to:** build and compose 2D rotation/homogeneous transform matrices in
NumPy; convert a point between two frames.

**Next week:** forward kinematics — applying these transforms to compute a robot arm's
end-effector position and a mobile robot's pose update.

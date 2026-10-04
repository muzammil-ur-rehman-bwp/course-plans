# Week 1 Summary — Introduction to Robotics

**Key takeaways:**
- Robots are classified as manipulators, mobile robots, aerial robots, or humanoids; this course
  focuses on mobile (differential-drive) robots and simple planar arms.
- The sense-plan-act architecture (sensors → perception → planning → control → actuators, with
  feedback) is the mental model used throughout the course; reactive control skips explicit
  planning.
- Python (`rclpy`) + ROS 2 + a simulator (Gazebo/Webots) is the course's toolchain; OpenCV joins
  later for vision (Week 11).

**You should now be able to:** classify a robot by type; describe the sense-plan-act loop;
verify your ROS 2/simulator environment is working.

**Next week:** the math foundations (coordinate frames, transformations) needed before we can
formalize robot motion.

# Week 9 Summary — Midterm + Sensors II: Range Sensing

**Key takeaways:**
- Midterm exam covered Weeks 1–8.
- LiDAR, ultrasonic, and IR sensors all measure distance but differ in range, precision, and
  cost; ROS 2 represents LiDAR scans via `sensor_msgs/LaserScan`.
- Converting a range scan into an occupancy grid reuses the frame-transform skills from Week 2:
  each (range, angle) reading becomes a point that must be transformed into the grid/world frame
  using the robot's pose.

**You should now be able to:** explain the tradeoffs between range sensor types; convert a
LiDAR scan into an occupancy grid.

**Next week:** feedback control — designing and tuning a PID controller.

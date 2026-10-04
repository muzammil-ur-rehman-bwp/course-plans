# Week 6 Summary — ROS 2 Continued: Services, Parameters, Simulation

**Key takeaways:**
- Services handle one-off request/response interactions; topics handle continuous streams.
- Parameters make nodes configurable without code changes; launch files start multiple
  configured nodes with one command.
- Gazebo/Webots bridge simulated sensors/actuators to ROS 2 topics (`/scan`, `/camera/...`,
  `/cmd_vel`), letting the same `rclpy` code drive a simulated robot as would drive a real one.
- `Twist` messages on `/cmd_vel` command linear and angular velocity.

**You should now be able to:** write a ROS 2 service and a parameterized node; write a launch
file; drive a simulated robot via `/cmd_vel`.

**Next week:** robot dynamics and actuators — the physical layer beneath these velocity commands.

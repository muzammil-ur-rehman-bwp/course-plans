# Week 5 Summary — Introduction to ROS 2

**Key takeaways:**
- ROS 2 organizes robot software as nodes communicating via topics (async pub/sub), services
  (sync request/response), and parameters (runtime configuration).
- Packages group related nodes/launch files; workspaces are built together with `colcon`.
- `rclpy` provides the Python API for creating publisher/subscriber nodes.
- CLI tools (`ros2 topic list/echo/hz`, `ros2 node list`) are essential for debugging a running
  ROS 2 system without writing extra code.

**You should now be able to:** create a ROS 2 package; write a publisher and subscriber node in
Python; inspect a running system with CLI tools.

**Next week:** services, parameters, launch files, and bringing up a simulated robot.

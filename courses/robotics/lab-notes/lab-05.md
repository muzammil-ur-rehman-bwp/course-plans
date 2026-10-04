# Lab Notes 5 — ROS 2 Publisher & Subscriber

**Concept recap:** publishers and subscribers communicate asynchronously via a named topic and
a shared message type; multiple subscribers can listen to the same topic independently.

**Common pitfalls:**
- Publisher and subscriber using different message types or topic names (even a typo) — they
  will simply never connect, with no error raised.
- Forgetting `rclpy.spin(node)`, so the node never processes incoming messages or timer
  callbacks.

**Debugging tip:** `ros2 topic info /topic_name` shows the message type and number of
publishers/subscribers currently connected — useful for confirming a mismatch.

**Instructor tip:** Task D (parameterized threshold subscriber) previews the parameter pattern
used more heavily in Week 6 — a good moment to re-emphasize `declare_parameter`/`get_parameter`.

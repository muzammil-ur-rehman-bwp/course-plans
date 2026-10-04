# Lab Notes 1 — Environment Setup & ROS 2 Hello World

**Concept recap:** a ROS 2 node is a single executable process participating in the ROS 2
graph; `ros2 node list`/`ros2 node info` are the first debugging tools to reach for when
something isn't connecting as expected.

**Common pitfalls:**
- Forgetting to `source install/setup.bash` (or the equivalent) in a new terminal — causes
  "package not found" errors even though the package built successfully.
- Simulator fails to open due to GPU/driver issues in some lab environments — have a
  CPU-rendering fallback flag documented for the specific simulator in use.

**Debugging tip:** if a node doesn't appear in `ros2 node list`, check the terminal it was
launched in for Python exceptions — a crashed node simply disappears rather than raising an
obvious top-level error.

**Instructor tip:** resolve environment setup issues individually before the lecture content
builds any further — nothing in Weeks 2+ labs works without a functioning ROS 2 + simulator
setup.

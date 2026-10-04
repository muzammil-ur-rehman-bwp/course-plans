# Lab Notes 6 — Services, Launch Files & Simulation

**Concept recap:** services block for a response (use sparingly, for one-off queries); launch
files start/configure multiple nodes together; `/cmd_vel` (`geometry_msgs/Twist`) is the
standard velocity-command interface for mobile robots in ROS 2.

**Common pitfalls:**
- Driving a "square" by only commanding forward motion without pausing to turn — a square path
  needs alternating straight segments and ~90-degree turns, each as separate timed commands.
- Launch file referencing an executable name that doesn't match `setup.py`'s entry points,
  causing a silent "executable not found" failure.

**Debugging tip:** if the simulated robot doesn't move, confirm with `ros2 topic echo /cmd_vel`
that commands are actually being published before assuming the simulator/robot model is at
fault.

**Instructor tip:** have students predict the square path's drift (it likely won't close
perfectly due to timing-based turns rather than feedback-based ones) — this sets up the Week 10
PID motivation nicely.

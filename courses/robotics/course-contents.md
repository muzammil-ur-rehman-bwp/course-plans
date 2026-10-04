# Course Contents: Robotics

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Tools used**.

---

## Week 1 — Introduction to Robotics
- **Topics:** What is a robot; history/milestones; taxonomy (manipulators, mobile robots, aerial,
  humanoid); the sense-plan-act (and reactive) architecture; course tooling overview.
- **Subtopics/Skills:** installing Python, ROS 2, and a simulator (Gazebo/Webots); verifying the
  dev environment.
- **Readings:** Siegwart et al. Ch. 1.
- **Software:** Python 3.10+, ROS 2 install, Gazebo/Webots install.

## Week 2 — Math Foundations: Frames & Transformations
- **Topics:** 2D/3D coordinate frames, rotation matrices, Euler angles (brief), homogeneous
  transformation matrices, composing transforms.
- **Subtopics/Skills:** implementing rotation/translation matrices in NumPy; transforming a point
  between two frames.
- **Readings:** Craig Ch. 2; Siegwart et al. Ch. 3 (intro).
- **Software:** NumPy.

## Week 3 — Forward Kinematics
- **Topics:** Degrees of freedom, joint types (revolute/prismatic), forward kinematics for a
  planar 2-/3-link arm; the kinematic model of a differential-drive mobile robot.
- **Subtopics/Skills:** computing end-effector position from joint angles in Python; simulating a
  differential-drive robot's pose update (`x, y, theta`) given wheel velocities.
- **Readings:** Craig Ch. 3; Siegwart et al. Ch. 3 (§3.2, differential drive).
- **Software:** NumPy, Matplotlib (for plotting robot pose).

## Week 4 — Inverse Kinematics
- **Topics:** Inverse kinematics problem statement; analytical IK for a 2-link planar arm;
  numerical/iterative IK (Jacobian-based, conceptual).
- **Subtopics/Skills:** solving IK analytically for a 2-link arm in Python; visualizing reachable
  workspace.
- **Readings:** Craig Ch. 4 (selected sections).
- **Software:** NumPy, Matplotlib.
- **Assignment 1 assigned** (kinematics).

## Week 5 — Introduction to ROS 2
- **Topics:** ROS 2 architecture (nodes, topics, messages), DDS middleware (conceptual), workspaces
  and packages, writing a publisher/subscriber node in Python (`rclpy`).
- **Subtopics/Skills:** creating a ROS 2 package; writing a minimal publisher and subscriber node;
  using `ros2 topic` CLI tools to inspect traffic.
- **Readings:** ROS 2 official "Beginner: Client libraries" tutorials.
- **Software:** ROS 2, `rclpy`.

## Week 6 — ROS 2 Continued: Services, Parameters, Simulation
- **Topics:** Services vs. topics (request/response), node parameters, launch files; bringing up
  a simulated robot in Gazebo/Webots and controlling it via ROS 2 topics.
- **Subtopics/Skills:** writing a simple service server/client; writing a launch file; driving a
  simulated differential-drive robot with velocity commands (`/cmd_vel`).
- **Readings:** ROS 2 official tutorials (services, launch); Gazebo/Webots ROS 2 integration docs.
- **Software:** ROS 2, Gazebo/Webots.

## Week 7 — Robot Dynamics & Actuators
- **Topics:** Mass, inertia, torque, force; basics of DC motors and servo motors; motor control
  signals (PWM, conceptual); gearing/torque-speed tradeoff.
- **Subtopics/Skills:** computing torque requirements for a simple arm/wheel; relating simulated
  actuator commands to physical motor behavior.
- **Readings:** Siegwart et al. Ch. 2 (actuation sections).
- **Software:** Python/NumPy (calculations only).

## Week 8 — Sensors I: Encoders, IMU, Odometry; Midterm Review
- **Topics:** Wheel encoders and odometry calculation; IMU basics (accelerometer/gyroscope);
  sources and accumulation of odometry error (drift); review session for Weeks 1–7.
- **Subtopics/Skills:** computing odometry (`x, y, theta`) from simulated encoder ticks; practice
  problems for the midterm.
- **Readings:** Siegwart et al. Ch. 5 (odometry sections).
- **Software:** Python, ROS 2 (`nav_msgs/Odometry` message, conceptual).

## Week 9 — Midterm Exam; Sensors II: Range Sensing
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: LiDAR/ultrasonic/IR range sensors; the
  occupancy grid representation of range data.
- **Readings:** Siegwart et al. Ch. 4 (range sensors).
- **Software:** ROS 2 (`sensor_msgs/LaserScan`), simulated LiDAR in Gazebo/Webots.

## Week 10 — Feedback Control: PID
- **Topics:** Open-loop vs. closed-loop control; PID controller structure (P, I, D terms); tuning
  heuristics; applying PID to a line/heading-following or set-point task.
- **Subtopics/Skills:** implementing a PID controller in Python/ROS 2 for a simulated robot
  (e.g., maintain heading or reach a goal distance); tuning gains and observing response.
- **Readings:** Siegwart et al. Ch. 6 (control sections, intro-level).
- **Software:** Python, ROS 2, Matplotlib (response plots).
- **Capstone project proposal due.**

## Week 11 — Computer Vision for Robotics
- **Topics:** Pinhole camera model (conceptual), image basics, OpenCV fundamentals (read/display/
  filter), color-based object detection, basic feature detection (edges/contours).
- **Subtopics/Skills:** writing an OpenCV pipeline to detect a colored object in a simulated
  camera feed; publishing detection results as a ROS 2 topic.
- **Readings:** Bradski & Kaehler, selected chapters; OpenCV-Python tutorials.
- **Software:** OpenCV (`opencv-python`), ROS 2 (`sensor_msgs/Image`, `cv_bridge`).
- **Assignment 2 assigned** (control + vision).

## Week 12 — State Estimation: Sensor Fusion & Kalman Filter Intro
- **Topics:** Why fuse sensors (noise, drift, partial observability); the Kalman filter concept
  (predict/update cycle) for a simple 1D estimation problem.
- **Subtopics/Skills:** implementing a 1D Kalman filter in NumPy to fuse a noisy position sensor
  with a motion model.
- **Readings:** Siegwart et al. Ch. 5 (Kalman filter intro).
- **Software:** NumPy, Matplotlib.

## Week 13 — Path Planning I: Grid-Based Planning
- **Topics:** Configuration space, occupancy grids, grid-based search for planning (BFS/A* applied
  to navigation), path cost vs. heuristic cost in a navigation context.
- **Subtopics/Skills:** implementing A* on an occupancy grid to plan a path between two points;
  visualizing the planned path over the grid.
- **Readings:** Siegwart et al. Ch. 6 (path planning, grid methods).
- **Software:** Python, NumPy, Matplotlib.
- **Assignment 3 assigned** (path planning).

## Week 14 — Path Planning II: Sampling-Based Planning & Obstacle Avoidance
- **Topics:** Limitations of grid search in high-dimensional/continuous spaces; Rapidly-exploring
  Random Trees (RRT) concept and basic implementation; reactive local obstacle avoidance
  (e.g., simple potential-field or bug algorithm, conceptual).
- **Subtopics/Skills:** implementing a basic RRT planner in 2D; comparing RRT vs. A* paths.
- **Readings:** Siegwart et al. Ch. 6 (sampling-based planning).
- **Software:** Python, NumPy, Matplotlib.

## Week 15 — Autonomous Navigation Pipeline
- **Topics:** Integrating perception (sensors/vision) → state estimation → planning → control into
  a single ROS 2 navigation pipeline; overview of the ROS 2 Navigation Stack (Nav2) as the
  production-grade analog of what students built by hand.
- **Subtopics/Skills:** assembling a minimal end-to-end pipeline in simulation: localize → plan →
  drive-to-goal while avoiding a simulated obstacle.
- **Readings:** ROS 2 Navigation (Nav2) documentation overview.
- **Software:** ROS 2, Gazebo/Webots, OpenCV (if vision used).

## Week 16 — Capstone Presentations, Course Review, Ethics & Safety
- **Topics:** Student capstone project presentations; recap of the course map (kinematics → ROS 2
  → sensing → control → vision → estimation → planning → navigation); robotics ethics and safety
  (human-robot interaction safety, autonomous decision-making, job/societal impact, data privacy
  from robot sensors).
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Mini-Project (introduced Week 9, proposal Week 10, final Week 16)
Students (individually or in small groups) design an autonomous behavior for a simulated robot
that integrates at least three course techniques (e.g., kinematics + PID control + path planning,
or vision-based detection + control). Deliverables: a short design report, the ROS 2/simulation
implementation, and a 5–7 minute live demo + presentation. Example topics: line-following robot,
obstacle-avoiding mobile robot, simple pick-and-place arm sequence, vision-guided goal-seeking
robot.

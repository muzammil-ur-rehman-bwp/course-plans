# Course Plan: Robotics

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Robotics |
| Level | Undergraduate (3rd/4th year, BS Computer Science / Electrical / Mechatronics / Software Engineering) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | Programming Fundamentals (Python), Linear Algebra, Basic Physics (mechanics), Data Structures (recommended) |
| Programming Language | Python 3.x (with ROS/ROS 2 Python client library `rclpy`) |
| Core Tools | Python, NumPy, Matplotlib, ROS 2, Gazebo/Webots (simulation), OpenCV |
| Duration | 16 teaching weeks (1 semester) + exam weeks |
| Delivery Mode | Lecture + Lab (simulation-first, with optional hardware kits e.g. differential-drive robot/Arduino) |

## 2. Course Description

This course introduces the fundamentals of robotics: kinematics, sensing, actuation, control, and
autonomous behavior. Python is used throughout — both as a standalone tool for kinematics/
control math (NumPy) and as the scripting language for the Robot Operating System (ROS 2) to
drive simulated (and optionally real) robots. The course moves from the mechanics of a single
robot arm/mobile robot to perception (sensors, basic computer vision) and finally to autonomous
navigation and planning, culminating in a capstone robot project in simulation.

## 3. Goals

- Understand the mathematical foundations of robot motion: coordinate frames, kinematics,
  transformations.
- Gain hands-on experience with ROS 2 as the industry-standard robotics middleware, using Python.
- Understand sensing (encoders, IMU, LiDAR, cameras) and basic perception pipelines.
- Implement classical control (PID) and basic autonomous navigation/path planning.
- Design, simulate, and present a capstone robot behavior.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall and explain core robotics terminology, robot types, and system architecture (sense-plan-act). | Remember, Understand |
| CLO2 | Apply coordinate transformations and forward/inverse kinematics to compute robot pose/joint configuration. | Apply |
| CLO3 | Analyze sensor data (encoders, IMU, LiDAR, camera) to estimate robot state. | Analyze |
| CLO4 | Implement a ROS 2 node graph (publishers/subscribers/services) in Python to control a simulated robot. | Apply, Analyze |
| CLO5 | Design and tune a PID controller for a robot motion task. | Apply, Evaluate |
| CLO6 | Implement a basic path-planning/navigation algorithm and evaluate its performance in simulation. | Apply, Analyze, Evaluate |
| CLO7 | Design, build, and present an original autonomous robot behavior (capstone) in simulation. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Foundation | 1–4 | Remember, Understand, Apply | Robotics intro, math foundations, ROS 2 basics |
| Core Mechanics | 5–8 | Apply, Analyze | Kinematics, dynamics, sensing |
| Control & Perception | 9–12 | Apply, Analyze, Evaluate | PID control, computer vision, state estimation |
| Autonomy & Synthesis | 13–16 | Analyze, Evaluate, Create | Path planning, navigation, capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Introduction to robotics: history, robot types, sense-plan-act architecture; dev environment setup | Remember, Understand |
| 2 | Math foundations: coordinate frames, rotation matrices, homogeneous transforms (NumPy) | Understand, Apply |
| 3 | Forward kinematics (2-link/3-link planar arm; differential-drive robot model) | Apply |
| 4 | Inverse kinematics (analytical & numerical approaches) | Apply, Analyze |
| 5 | Intro to ROS 2: nodes, topics, publishers/subscribers, `rclpy` | Understand, Apply |
| 6 | ROS 2 continued: services, parameters, launch files; simulating a robot in Gazebo/Webots | Apply |
| 7 | Robot dynamics basics (mass, inertia, torque) and actuators (DC motors, servos) | Understand, Apply |
| 8 | Sensors I: encoders, IMU, odometry; Midterm review | Understand, Apply |
| 9 | **Midterm Exam** + Sensors II: LiDAR and range sensing basics | Remember–Apply |
| 10 | Feedback control: PID controller design and tuning | Apply, Analyze |
| 11 | Computer vision for robotics: camera model, OpenCV basics, color/feature detection | Apply, Analyze |
| 12 | State estimation intro: sensor fusion concept, Kalman filter (conceptual + simple 1D implementation) | Analyze |
| 13 | Path planning I: configuration space, grid-based planning (BFS/A* on occupancy grid) | Apply, Analyze |
| 14 | Path planning II: sampling-based planning (RRT) and local obstacle avoidance | Analyze, Evaluate |
| 15 | Autonomous navigation: integrating perception + planning + control in a ROS 2 pipeline | Analyze, Evaluate |
| 16 | Capstone project presentations; course review; robotics ethics & safety | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 20% | ROS 2/simulation labs, submitted weekly |
| Assignments (4) | 20% | Problem sets tied to Weeks 4, 8, 11, 14 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Capstone Mini-Project | 20% | Proposal (Wk 10) + implementation + presentation (Wk 16) |
| Final Exam | 15% | Comprehensive, emphasis on Weeks 9–16 |

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency.

## 8. Tools & Software

- Python 3.10+, NumPy, Matplotlib
- ROS 2 (Humble or newer) with `rclpy`
- Gazebo or Webots for simulation (no physical hardware required; optional hardware kit extension)
- OpenCV (`opencv-python`)
- Git/GitHub for lab and project submission

## 9. Reference Textbooks

- Siegwart, R., Nourbakhsh, I. & Scaramuzza, D. — *Introduction to Autonomous Mobile Robots*.
- Craig, J. — *Introduction to Robotics: Mechanics and Control* (kinematics chapters).
- Official ROS 2 documentation and tutorials (docs.ros.org).
- Bradski, G. & Kaehler, A. — *Learning OpenCV* (relevant chapters for vision labs).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done in
pairs/small groups with clearly attributed contributions. Code plagiarism (including uncredited
AI-generated code submitted as original work) is handled per institutional academic integrity
policy.

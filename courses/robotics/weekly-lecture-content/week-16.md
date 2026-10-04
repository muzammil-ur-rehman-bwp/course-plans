# Week 16 — Lecture Content: Capstone, Course Review, Ethics & Safety

## 1. Capstone Presentations
Each student/team presents (5–7 minutes + Q&A, with a live or recorded simulation demo): the
robot behavior designed, the techniques applied (from the course), evaluation results, and
lessons learned. See `assignments/capstone-rubric.md` for grading criteria.

## 2. Course Review — The Big Picture
```
Math foundations: frames & transforms (Week 2)
        |
        v
Kinematics: forward & inverse (Weeks 3-4)
        |
        v
ROS 2 middleware (Weeks 5-6) + Dynamics/actuators (Week 7)
        |
        v
Sensing: encoders/IMU/odometry, range sensors (Weeks 8-9)
        |
        v
Control (PID, Week 10) + Vision (Week 11) + State estimation (Week 12)
        |
        v
Path planning: grid-based & sampling-based (Weeks 13-14)
        |
        v
Integration: full navigation pipeline (Week 15)
        |
        v
Capstone: an original autonomous robot behavior (Week 16)
```
Each phase's tools (transforms, kinematics equations, ROS 2 topics/services) reappear directly
in later weeks — the course is cumulative, not a sequence of disconnected topics.

## 3. Robotics Ethics and Safety
- **Human-robot interaction safety**: physical robots can cause harm; even in simulation,
  designing with safety margins (speed limits, obstacle stopping distance) is a professional
  habit to build now.
- **Autonomous decision-making**: as robots act with less direct human oversight, understanding
  and being able to explain *why* a robot took an action becomes more important, not less.
- **Societal/job impact**: automation changes labor markets; engineers should be aware of this
  context, not just the technical challenge.
- **Data privacy**: robot sensors (especially cameras) can capture sensitive information about
  people and spaces; understand what data your system collects and how it's handled.

## 4. Closing Discussion
Open discussion: where could each capstone project go next (real hardware deployment, more
sophisticated planning, learned perception models)?

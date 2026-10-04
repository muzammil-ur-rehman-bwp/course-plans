# Week 10 Lecture Plan — Robotics
## Topic: Feedback Control — PID

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the difference between open-loop and closed-loop control. (*Understand*)
2. Apply a PID controller to a simulated robot control task. (*Apply*)
3. Evaluate and tune PID gains based on observed response behavior. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Open-loop vs. closed-loop | Examples, why feedback matters |
| 0:25–1:00 | PID controller structure | P, I, D terms explained individually, live demo |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | Tuning heuristics | Effect of each gain on response (overshoot, settling time) |
| 1:40–2:00 | Worked example | Tune a PID controller for a heading-hold task in simulation |

### Materials/Equipment
- Live-coding environment, ROS 2, Matplotlib (response plots)

### Formative Check (in-class)
Exercise: given a response plot showing oscillation, propose which PID gain to adjust and in
which direction.

### Link to Lab/Assessment
Lab 10: implement and tune a PID controller for a simulated robot's heading/set-point task.
**Capstone project proposal due this week.**

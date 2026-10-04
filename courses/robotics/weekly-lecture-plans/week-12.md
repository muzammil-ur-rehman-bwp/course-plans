# Week 12 Lecture Plan — Robotics
## Topic: State Estimation — Sensor Fusion & Kalman Filter Intro

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why sensor fusion is needed (noise, drift, partial observability). (*Understand*)
2. Apply a 1D Kalman filter to fuse a noisy sensor with a motion model. (*Apply*)
3. Analyze filter output vs. raw sensor data to assess noise reduction. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why fuse sensors? | Odometry drift + noisy sensors motivate fusion |
| 0:25–1:00 | Kalman filter concept | Predict/update cycle, 1D derivation at a conceptual level |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | Implementation | Live-coded 1D Kalman filter in NumPy |
| 1:40–2:00 | Evaluation | Compare filtered vs. raw noisy signal on a plot |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib

### Formative Check (in-class)
Exercise: adjust the filter's process/measurement noise parameters and observe the effect on
smoothing vs. responsiveness.

### Link to Lab/Assessment
Lab 12: implement a 1D Kalman filter fusing a simulated noisy position sensor with a motion
model.

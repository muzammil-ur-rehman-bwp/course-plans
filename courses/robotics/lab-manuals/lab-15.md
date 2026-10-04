# Lab Manual 15 — End-to-End Navigation Pipeline

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture; reuse all prior labs

## Objectives
Assemble and test a minimal end-to-end navigation pipeline in simulation.

## Setup
Create `lab15_pkg`, importing/reusing components from Labs 9 (occupancy grid), 10 (PID), 12
(Kalman filter, optional), and 13/14 (planner of choice).

## Procedure
1. **Task A — Sensing & estimation:** wire up subscriptions to `/scan` and `/odom`; maintain a
   current pose estimate (raw odometry, or Kalman-filtered if using Task's optional fusion).
2. **Task B — Planning:** build the occupancy grid and run A* (or RRT) to the goal.
3. **Task C — Control:** feed the next waypoint as the PID controller's setpoint; publish
   `/cmd_vel` commands.
4. **Task D — Full run:** run the assembled pipeline in simulation to drive the robot from a
   start position to a goal while avoiding at least one obstacle; record a short video/screenshot
   sequence.
5. **Task E — Reflection:** identify the weakest link in your pipeline (least robust component)
   and propose one concrete improvement.

## Expected Output
A working package demonstrating Tasks A–D, plus the Task E reflection.

## Submission
Submit the package + reflection by the end of the lab session.

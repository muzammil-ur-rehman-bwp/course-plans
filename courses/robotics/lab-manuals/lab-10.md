# Lab Manual 10 — PID Controller Design & Tuning

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement and tune a PID controller for a simulated robot heading/set-point task.

## Setup
Create `lab10.ipynb` or a ROS 2 node in `lab10_pkg`.

## Procedure
1. **Task A — Implementation:** implement `PIDController` as shown in lecture.
2. **Task B — P-only tuning:** set `Ki = Kd = 0`; tune `Kp` alone for a heading-hold task; plot
   the response and note any steady-state error/oscillation.
3. **Task C — Full PID tuning:** add `Kd` to dampen oscillation, then a small `Ki` to remove
   steady-state error; plot the improved response.
4. **Task D — Comparison:** present P-only vs. full PID response plots side by side with a short
   written comparison.

## Expected Output
A notebook/package with Tasks A–D, including all response plots.

## Submission
Submit the notebook/package by the end of the lab session. **Capstone project proposal also due
this week.**

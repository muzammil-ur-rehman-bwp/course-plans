# Lab Manual 8 — Odometry from Encoder Data

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Compute odometry from simulated encoder ticks and visualize accumulated drift.

## Setup
Create `lab08.ipynb`; use instructor-provided simulated encoder tick data for a short robot
trajectory.

## Procedure
1. **Task A — Ticks to distance:** implement `ticks_to_distance`; convert the provided tick
   sequence to per-wheel distances.
2. **Task B — Odometry integration:** using the differential-drive pose update (Week 3/8),
   integrate the per-wheel distances into an estimated trajectory.
3. **Task C — Drift visualization:** plot the estimated trajectory against the provided ground
   truth trajectory; report the final position error.
4. **Task D — Reflection:** identify which segment of the trajectory (straight vs. turning)
   accumulated the most error, and propose why.

## Expected Output
A notebook with Tasks A–D.

## Submission
Submit `lab08.ipynb` by the end of the lab session. **Assignment 1 is also due this week.**

# Lab Manual 9 — LiDAR Scan to Occupancy Grid

**Duration:** 3 hours (shortened due to midterm earlier in the week) | **Prerequisite:** Week 9 lecture

## Objectives
Convert a simulated LiDAR scan into an occupancy grid.

## Setup
Create `lab09.ipynb`; use the instructor-provided simulated LiDAR scan data (ranges + angle
parameters) and robot pose.

## Procedure
1. **Task A — Scan to points:** convert the raw scan (ranges + angles) into `(x, y)` points in
   the robot's local frame.
2. **Task B — Frame transform:** using Lab 2's transform functions, convert those points into
   the world frame given the robot's pose.
3. **Task C — Occupancy grid:** implement `scan_to_occupancy` and build the grid from the
   transformed points.
4. **Task D — Visualization:** display the occupancy grid as an image, with the robot's position
   marked.

## Expected Output
A notebook with Tasks A–D; this lab is intentionally lighter given the midterm earlier in the
week.

## Submission
Submit `lab09.ipynb` by the end of the lab session.

# Lab Manual 2 — Coordinate Frames & Transformations

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement and compose 2D rotation/homogeneous transform matrices in NumPy.

## Setup
Create `lab02.ipynb`.

## Procedure
1. **Task A — Rotation matrix:** implement `rotation_matrix_2d(theta)`; verify it correctly
   rotates the point `(1, 0)` by 90 degrees.
2. **Task B — Homogeneous transform:** implement `homogeneous_transform_2d(theta, tx, ty)` and
   `transform_point`; verify on 2 test cases with known expected output.
3. **Task C — Composing transforms:** given `T_world_robot` and `T_robot_sensor` (provided
   values), compute `T_world_sensor` and transform a point from the sensor frame to world frame.
4. **Task D — Mini-challenge:** given a point in the world frame, compute its coordinates in the
   sensor frame (i.e., invert the composed transform).

## Expected Output
A notebook with Tasks A–D, including verification against the provided known-answer test cases.

## Submission
Submit `lab02.ipynb` by the end of the lab session.

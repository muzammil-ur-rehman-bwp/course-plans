# Lab Manual 14 — RRT Planning & Comparison with A*

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Implement a basic RRT planner and compare it against the Lab 13 A* planner.

## Setup
Create `lab14.ipynb`; reuse the obstacle map/occupancy grid from Lab 13.

## Procedure
1. **Task A — RRT implementation:** implement `rrt` as shown in lecture, with an
   `obstacle_check` function based on the occupancy grid.
2. **Task B — Planning:** plan a path between the same start/goal used in Lab 13 with RRT.
3. **Task C — Comparison:** compare RRT's path (length, smoothness) and runtime against A*'s
   path from Lab 13.
4. **Task D — Parameter sensitivity:** re-run RRT with different `step_size`/`max_iter` values
   and report the effect on path quality and runtime.

## Expected Output
A notebook with Tasks A–D, including the A*-vs-RRT comparison.

## Submission
Submit `lab14.ipynb` by the end of the lab session.

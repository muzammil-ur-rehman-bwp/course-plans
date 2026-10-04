# Assignment 3 — Path Planning (Weeks 13–14)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 13 | **Due:** Start of Week 15

## Instructions
Submit `assignment03.ipynb` with working code and written answers for all questions, using the
provided occupancy grid `assignment03_grid.npy`.

## Questions
1. **(A\* planning, 20 pts)** Implement A* and plan a path between 2 given start/goal pairs on
   the provided grid; report path length and nodes expanded for each.
2. **(Heuristic comparison, 20 pts)** Re-run both planning problems with Manhattan vs. Euclidean
   heuristics; compare results in a table and explain any differences observed.
3. **(RRT planning, 20 pts)** Implement RRT and plan the same 2 start/goal pairs; report path
   length and runtime for each.
4. **(A\* vs. RRT, 20 pts)** For both start/goal pairs, compare A* and RRT results (path length,
   runtime, nodes/samples used) in a table.
5. **(Recommendation, 20 pts)** Based on your results, recommend which planner to use for this
   grid size/obstacle density, and describe one scenario (e.g., larger map, higher-DOF robot)
   where your recommendation would change.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

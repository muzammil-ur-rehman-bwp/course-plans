# Lab Manual 4 — Informed Search: A*, Hill Climbing, Simulated Annealing

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture; `Problem` class from Lab 3

## Objectives
Implement A* with a heuristic on a grid; implement hill climbing and simulated annealing for a
toy optimization problem.

## Setup
Create `lab04.ipynb`; reuse the grid/maze `Problem` class from Lab 3.

## Procedure
1. **Task A — Heuristic:** implement `manhattan_distance(a, b)` and verify by hand on 2–3 pairs
   of grid cells that it never overestimates the true shortest-path cost (admissibility check).
2. **Task B — A\*:** implement `a_star(problem, heuristic)`; solve the Lab 3 maze and compare
   nodes expanded against BFS/UCS from Lab 3.
3. **Task C — Hill climbing:** define a simple 1-D or 2-D objective function with at least one
   local optimum (e.g., a function with two peaks); implement `hill_climbing` and show it can
   get stuck at the lower peak from a bad starting point.
4. **Task D — Simulated annealing:** implement `simulated_annealing` on the same objective
   function; run it multiple times from the same bad starting point and report how often it
   finds the global optimum vs. hill climbing.

## Expected Output
A notebook with Tasks A–D; A* must find the same optimal cost as UCS (Lab 3) on the shared maze;
the simulated-annealing experiment must include at least 10 repeated trials.

## Submission
Submit `lab04.ipynb` by the end of the lab session.

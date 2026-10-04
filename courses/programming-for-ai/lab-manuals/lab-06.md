# Lab Manual 6 — Informed Search: A*

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture; reuse `MazeProblem` from Lab 5

## Objectives
Design a heuristic and implement A* search; compare it against BFS from Lab 5.

## Setup
Continue from Lab 5's `lab05.ipynb` or start a new `lab06.ipynb` importing the `MazeProblem`
class.

## Procedure
1. **Task A — Heuristic design:** implement a Manhattan-distance heuristic for the maze; argue
   in a markdown cell why it is admissible.
2. **Task B — A\* implementation:** implement `a_star(problem, h)` using `heapq`; solve the same
   maze(s) from Lab 5.
3. **Task C — Comparison:** compare path length and nodes expanded for BFS (Lab 5) vs. A*
   (this lab) on the same maze(s); present as a small table.
4. **Task D — Mini-challenge:** implement Greedy Best-First Search and add it to the comparison
   table; discuss any case where it returns a longer path than A*.

## Expected Output
A notebook with Tasks A–D, including the comparison table from Task C/D.

## Submission
Submit `lab06.ipynb` by the end of the lab session.

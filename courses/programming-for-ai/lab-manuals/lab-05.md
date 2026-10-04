# Lab Manual 5 — Search: BFS & DFS

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture; reuse `Graph` class from Lab 2

## Objectives
Implement a generic search `Problem` class and solve a maze/8-puzzle with BFS and DFS.

## Setup
Create `lab05.ipynb`; import/paste your `Graph` class from Lab 2 or the instructor-provided maze
representation (2D grid with walls).

## Procedure
1. **Task A — Problem formulation:** implement a `MazeProblem` class with `actions`, `result`,
   `is_goal` for a given 2D grid maze (provided as a text file / list of strings).
2. **Task B — BFS:** implement `bfs(problem)` and solve the maze; print the solution path length.
3. **Task C — DFS:** implement `dfs(problem)` and solve the same maze; compare path length to BFS.
4. **Task D — Analysis:** instrument both algorithms to count nodes expanded; report and briefly
   explain the difference between BFS and DFS results (optimality, nodes expanded).

## Expected Output
A notebook with Tasks A–D; both solvers must work on at least 2 different provided mazes.

## Submission
Submit `lab05.ipynb` by the end of the lab session.

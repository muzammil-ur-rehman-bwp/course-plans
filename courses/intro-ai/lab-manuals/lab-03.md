# Lab Manual 3 — Uninformed Search: BFS, DFS, UCS

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Implement a generic search `Problem` class and solve a maze/graph with BFS, DFS, and
uniform-cost search.

## Setup
Create `lab03.ipynb`; use the instructor-provided maze representation (2D grid with walls) or a
small weighted graph (adjacency dict with edge costs).

## Procedure
1. **Task A — Problem formulation:** implement a `MazeProblem` (or `GraphProblem`) class with
   `actions`, `result`, `is_goal`, and `step_cost` for the given maze/graph.
2. **Task B — BFS & DFS:** implement `bfs(problem)` and `dfs(problem)`; solve the same maze with
   both; print the solution path length for each.
3. **Task C — UCS:** implement `uniform_cost_search(problem)` using `heapq`; solve a weighted
   version of the problem and report the optimal path cost.
4. **Task D — Analysis:** instrument all three algorithms to count nodes expanded; report nodes
   expanded and path length/cost for each, and briefly explain any differences (optimality,
   nodes expanded) in 3–5 sentences.

## Expected Output
A notebook with Tasks A–D; all three solvers must work on at least 2 different provided
mazes/graphs.

## Submission
Submit `lab03.ipynb` by the end of the lab session.

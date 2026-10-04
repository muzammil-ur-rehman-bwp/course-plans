# Lab Manual 5 — Rigorous CSP & Simulated Annealing

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement AC-3 with heuristic-ordered backtracking for a CSP, and simulated annealing for a toy
combinatorial optimization problem.

## Setup
1. Reuse your course virtual environment.
2. Create `lab05.ipynb`.

## Procedure
1. **Task A — AC-3:** implement `ac3`/`revise` from the Week 5 lecture content for a graph-
   coloring CSP (e.g., coloring the map of Australia with 3 colors); report which domains are
   pruned before any search begins.
2. **Task B — Heuristic backtracking:** implement backtracking search with MRV variable
   ordering and forward checking for the same CSP; report the number of backtracks with and
   without MRV to show the heuristic's effect.
3. **Task C — Simulated annealing:** implement `simulated_annealing` from the Week 5 lecture
   content for a 12-city random TSP instance with a geometric cooling schedule; plot tour cost
   vs. iteration.
4. **Task D — Mini-challenge:** re-run Task C with a much faster cooling schedule (e.g., halving
   the number of steps) and a much slower one; compare final tour quality across the three runs.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the Task C/D plots.

## Submission
Export/submit `lab05.ipynb` via the course submission system by the end of the lab session.

# Lab Manual 7 — CSP & Local Search

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Solve N-Queens via backtracking CSP and via simulated annealing; compare the two approaches.

## Setup
Create `lab07.ipynb`.

## Procedure
1. **Task A — CSP formulation:** define variables/domains/constraints for N-Queens (N=8) as a
   CSP.
2. **Task B — Backtracking:** implement backtracking search with forward checking; solve N=8 and
   report the solution and runtime.
3. **Task C — Simulated annealing:** implement simulated annealing for N-Queens (cost = number of
   attacking pairs); solve N=8 and report the solution and runtime.
4. **Task D — Comparison:** run both approaches for N=8, 12, 16; compare runtime and solution
   quality (did simulated annealing always find a 0-conflict solution?) in a short table.

## Expected Output
A notebook with Tasks A–D, including the comparison table from Task D.

## Submission
Submit `lab07.ipynb` by the end of the lab session.

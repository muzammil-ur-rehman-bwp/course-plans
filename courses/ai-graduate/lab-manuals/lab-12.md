# Lab Manual 12 — Empirical Complexity: Runtime Scaling

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Empirically demonstrate how a complete SAT/CSP solver's runtime scales with problem size and
difficulty, connecting worst-case complexity results to observed behavior.

## Setup
1. Reuse your course virtual environment and Week 6's `dpll` implementation.
2. Create `lab12.ipynb`.

## Procedure
1. **Task A — Random 3-SAT generator:** implement `random_3sat` from the Week 12 lecture
   content; generate instances at a fixed number of variables (e.g., 20) across clause/variable
   ratios from 2.0 to 6.0 in steps of 0.5.
2. **Task B — Runtime sweep:** implement `time_dpll`; run it across the ratio sweep (5 trials
   per ratio) and plot average runtime vs. ratio.
3. **Task C — Satisfiability-threshold identification:** from the Task B plot, identify the
   ratio with the highest average runtime and compare it to the known empirical threshold
   (~4.3) discussed in the Week 12 lecture content.
4. **Task D — Mini-challenge:** repeat the sweep at a larger problem size (e.g., 30 variables)
   and discuss how both the location and the sharpness of the difficulty spike change as problem
   size grows.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and both required plots.

## Submission
Export/submit `lab12.ipynb` via the course submission system by the end of the lab session.

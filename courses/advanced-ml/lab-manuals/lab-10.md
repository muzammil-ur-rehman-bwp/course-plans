# Lab Manual 10 — Density-Ratio Estimation and the Overlap Failure Mode

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Estimate covariate-shift density-ratio weights via a classifier, and empirically demonstrate the
overlap-driven variance blow-up and its effect on importance-weighted estimation.

## Setup
1. Reuse your virtual environment.
2. Create `lab10.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `covariate_shift_experiment` exactly as in the Week 10
   lecture content, for `shift` $\in\{0.5,1.5,3.0,5.0\}$.
2. **Task B — Weight diagnostics table:** tabulate mean, variance, and max of the estimated
   weights $w(x)$ at each shift level, confirming all three grow with increasing shift.
3. **Task C — Downstream estimation comparison:** fit (a) an unweighted and (b) an importance-
   weighted linear regression on training data at `shift=3.0`, evaluate both on freshly sampled
   test data at the same shift, and report which performs better.
4. **Task D — Mini-challenge:** repeat Task C at `shift=0.5` (good overlap) and `shift=5.0` (poor
   overlap), and report how the weighted model's advantage over the unweighted model changes
   across these two regimes, relating the result to the weight-variance diagnostic from Task B.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required tables.

## Submission
Export/submit `lab10.ipynb` via the course submission system by the end of the lab session.

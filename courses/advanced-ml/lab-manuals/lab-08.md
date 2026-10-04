# Lab Manual 8 — Propensity-Score Estimation and IPW-Based ATE Estimation

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement propensity-score estimation and inverse-propensity-weighted (IPW) ATE estimation on
simulated observational data with a known confounder, and empirically confirm IPW corrects the
naive estimator's bias.

## Setup
1. Reuse your virtual environment.
2. Create `lab08.ipynb`.

## Procedure
1. **Task A — Implementation:** implement the simulation and estimators exactly as in the Week 8
   lecture content (naive difference-in-means, logistic-regression propensity estimation, IPW
   ATE).
2. **Task B — Bias comparison:** run the simulation and report the naive estimate, the IPW
   estimate, and the true ATE; confirm IPW is substantially closer to the truth.
3. **Task C — Repeated-trial variance:** repeat the simulation across 200 independent datasets
   and report the mean and standard deviation of both estimators' errors, confirming IPW's mean
   error is much smaller (even if its variance is somewhat higher than the naive estimator's).
4. **Task D — Mini-challenge:** repeat the poor-overlap variant from the Week 8 in-class exercise
   (`true_propensity = 1/(1+exp(-(3*X)))`) and report how IPW's error distribution across 200
   trials changes compared to Task C's good-overlap case.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required summary statistics.

## Submission
Export/submit `lab08.ipynb` via the course submission system by the end of the lab session.

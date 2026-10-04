# Lab Manual 12 — Median-of-Means and Trimmed-Mean Robust Estimation

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement trimmed-mean and median-of-means estimators and empirically compare their error to the
sample mean under heavy-tailed and adversarially contaminated data.

## Setup
1. Reuse your virtual environment.
2. Create `lab12.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `trimmed_mean`, `median_of_means`, and `contaminate`
   exactly as in the Week 12 lecture content.
2. **Task B — Heavy-tailed comparison:** run the heavy-tailed (Pareto-adjacent) experiment and
   report all three estimators' errors.
3. **Task C — Contamination comparison:** run the adversarial-contamination experiment at
   $\epsilon=0.05$ and report all three estimators' errors, confirming the sample mean's error is
   dramatically larger.
4. **Task D — Breakdown-point sweep:** sweep $\epsilon\in\{0.01,0.05,0.1,0.2,0.4\}$, plot all
   three estimators' errors against $\epsilon$ (log scale for the sample-mean error), and
   identify the $\epsilon$ at which the trimmed mean (breakdown point 0.1 as implemented) itself
   starts to fail.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required plot.

## Submission
Export/submit `lab12.ipynb` via the course submission system by the end of the lab session.

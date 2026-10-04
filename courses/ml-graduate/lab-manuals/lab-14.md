# Lab Manual 14 — Bias-Variance Decomposition and Information Criteria

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Empirically decompose squared-error risk into bias² and variance, and compute AIC/BIC for a
family of nested models.

## Setup
Create `lab14.ipynb`. NumPy, SciPy, Matplotlib.

## Procedure
1. **Task A — Decomposition sweep:** implement the polynomial-degree bias-variance sweep from the
   lecture content for degrees $1,2,3,5,9,15$; plot bias², variance, and their sum vs. degree.
2. **Task B — Verify the decomposition:** confirm numerically that
   $\mathrm{bias}^2+\mathrm{variance}+\sigma^2 \approx$ the directly-measured mean squared error,
   for each degree.
3. **Task C — AIC/BIC:** fit the same family of polynomial models by maximum likelihood (Gaussian
   noise model) on a single sample; compute AIC and BIC for each degree; plot both criteria vs.
   degree.
4. **Task D — Model selection:** report which degree AIC selects and which BIC selects; discuss
   any difference in light of Week 14's consistency-vs-predictive-optimality framing.
5. **Task E — Cross-validation comparison:** compute $k$-fold cross-validated MSE for the same
   model family; compare the model it selects to AIC's and BIC's choices.

## Expected Output
A notebook with Tasks A–E and all required plots/tables.

## Submission
Submit `lab14.ipynb` by the end of the lab session.

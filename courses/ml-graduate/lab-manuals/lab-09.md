# Lab Manual 9 — Bayesian Linear Regression From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement Bayesian linear regression from scratch and verify the MAP-estimate/ridge-regression
equivalence.

## Setup
Create `lab09.ipynb`. NumPy, scikit-learn (cross-check only).

## Procedure
1. **Task A — Posterior implementation:** implement `bayesian_linear_regression` (posterior mean
   and covariance) as in the lecture content.
2. **Task B — MAP vs. Ridge:** compare the posterior mean against `sklearn.linear_model.Ridge`
   with $\lambda=\sigma^2/\tau^2$; report the max absolute coefficient difference.
3. **Task C — Posterior uncertainty:** plot the posterior standard deviation of each weight as
   the training-set size grows ($n=5,20,50,200$); confirm uncertainty shrinks with more data.
4. **Task D — Predictive distribution:** compute and plot the posterior predictive mean and
   $\pm2\sigma$ band for a 1-D regression toy dataset.
5. **Task E — Prior sensitivity:** repeat Task B/D for three different $\tau^2$ values (tight,
   moderate, very loose prior) and discuss the effect on both the point estimate and the band
   width.

## Expected Output
A notebook with Tasks A–E and the plots from Tasks C, D, and E.

## Submission
Submit `lab09.ipynb` by the end of the lab session.

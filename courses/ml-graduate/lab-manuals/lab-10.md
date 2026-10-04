# Lab Manual 10 — Gaussian Process Regression From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement Gaussian Process regression from scratch using a numerically stable Cholesky-based
solve, and study the effect of kernel hyperparameters on the posterior.

## Setup
Create `lab10.ipynb`. NumPy, SciPy (`scipy.linalg.cho_factor`/`cho_solve`), Matplotlib.

## Procedure
1. **Task A — Implementation:** implement `gp_predict` as in the lecture content, using Cholesky
   decomposition (not `np.linalg.inv`).
2. **Task B — Posterior visualization:** plot the predictive mean and $\pm2\sigma$ band on toy
   1-D sine data with noise.
3. **Task C — Length-scale sweep:** repeat Task B for three RBF length scales (too short, about
   right, too long); discuss the qualitative difference in the fitted mean.
4. **Task D — Numerical instability demo:** deliberately use `np.linalg.inv(K)` instead of
   Cholesky on a case with many closely-spaced training points and a short length scale; compare
   the result (or failure) to the Cholesky-based solution.
5. **Task E — Reflection:** connect the GP predictive mean's form back to Week 7's representer
   theorem and Week 9's ridge-regression-as-MAP connection, in 3–4 sentences.

## Expected Output
A notebook with Tasks A–E, including all required plots.

## Submission
Submit `lab10.ipynb` by the end of the lab session.

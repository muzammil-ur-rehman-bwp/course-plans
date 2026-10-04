# Lab Manual 7 — Kernel Ridge Regression via the Representer Theorem

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Implement kernel ridge regression from scratch using the representer theorem's finite-dimensional
reduction, and cross-check against scikit-learn.

## Setup
Create `lab07.ipynb`. NumPy, SciPy, scikit-learn (cross-check only).

## Procedure
1. **Task A — Kernel matrix:** implement the RBF kernel and verify a given Gram matrix is positive
   semi-definite (check all eigenvalues $\geq -\text{tiny tolerance}$).
2. **Task B — Representer-theorem solve:** implement `kernel_ridge_fit`/`predict` as in the
   lecture content.
3. **Task C — Cross-check:** compare predictions against `sklearn.kernel_ridge.KernelRidge` on
   the same toy 1-D dataset; report the max absolute difference.
4. **Task D — Vary $\lambda$:** plot the fitted curve for three values of $\lambda$ (under-,
   well-, and over-regularized) and discuss the qualitative difference.
5. **Task E — Reflection:** explain, referencing the representer-theorem proof, why this
   finite-dimensional solve is guaranteed to recover the true infinite-dimensional RKHS optimum.

## Expected Output
A notebook with Tasks A–E and the three-curve plot from Task D.

## Submission
Submit `lab07.ipynb` by the end of the lab session.

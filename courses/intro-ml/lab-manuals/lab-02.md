# Lab Manual 2 — Linear Regression From Scratch and with scikit-learn

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement gradient descent for linear regression from scratch, and compare it against
scikit-learn's normal-equation solution.

## Setup
Continue with NumPy/scikit-learn; create `lab02.ipynb`.

## Procedure
1. **Task A — From scratch:** implement `fit_gradient_descent(X, y, alpha, n_iters)` as shown in
   lecture; fit it on the provided regression dataset and record the final cost.
2. **Task B — scikit-learn:** fit `LinearRegression` on the same data; print its coefficients and
   intercept.
3. **Task C — Compare:** compare your from-scratch `theta` to scikit-learn's `coef_`/`intercept_`;
   they should closely agree. If they don't, try scaling features and/or more iterations.
4. **Task D — Diagnostics:** plot the cost vs. iteration curve for gradient descent (confirm it
   decreases monotonically), and produce a residual plot for the scikit-learn model.

## Expected Output
A notebook with Tasks A–D, including the cost curve and residual plot.

## Submission
Submit `lab02.ipynb` by the end of the lab session.

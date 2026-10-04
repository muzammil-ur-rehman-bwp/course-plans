# Lab Manual 3 — Ridge, Lasso, and Elastic Net Regression

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Fit and compare Ridge, Lasso, and Elastic Net regression models, with correctly scaled features.

## Setup
Continue with scikit-learn; create `lab03.ipynb`.

## Procedure
1. **Task A — Baseline:** fit an unregularized `LinearRegression` on the provided
   many-feature dataset; report train and test MSE (expect a visible train/test gap if
   overfitting).
2. **Task B — Ridge:** build a `Pipeline` with `StandardScaler` + `Ridge`; fit at `alpha` in
   `{0.01, 1, 10, 100}`; report test MSE for each and plot the coefficient path.
3. **Task C — Lasso:** repeat Task B with `Lasso`; report the number of zeroed coefficients at
   each `alpha`, and identify which features are consistently kept.
4. **Task D — Elastic Net:** fit `ElasticNet` with `l1_ratio=0.5` at your best `alpha` from Task
   B/C; compare its test MSE to Ridge and Lasso at their best `alpha`.

## Expected Output
A notebook with Tasks A–D, including the coefficient-path plot and a short comparison table of
test MSE across models.

## Submission
Submit `lab03.ipynb` by the end of the lab session.

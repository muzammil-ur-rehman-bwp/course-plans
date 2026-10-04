# Assignment 1 — Regression & Regularization (Week 3)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 3 | **Due:** Start of Week 5

## Instructions
Submit `assignment01.ipynb` with working code and written answers for all questions, using the
provided dataset `assignment01_data.csv` (a regression dataset with several correlated numeric
features).

## Questions
1. **(Setup, 10 pts)** Load the dataset, split into train/test (80/20, `random_state=42`), and
   report the shape of each split.
2. **(Baseline, 15 pts)** Fit `LinearRegression` on the raw features; report train and test MSE
   and `R^2`. Comment on whether there is a meaningful train/test gap.
3. **(Gradient descent, 20 pts)** Implement batch gradient descent from scratch in NumPy for this
   dataset; report the final cost and compare your learned coefficients to scikit-learn's
   `coef_`/`intercept_` from Question 2.
4. **(Regularization, 25 pts)** Fit `Ridge` and `Lasso` (each inside a `StandardScaler` pipeline)
   across `alpha` in `{0.01, 0.1, 1, 10, 100}`; report test MSE for each and plot the Ridge
   coefficient path. State how many Lasso coefficients are zeroed out at `alpha=10`.
5. **(Recommendation, 30 pts)** Recommend one model (plain linear regression, Ridge, or Lasso,
   at a specific `alpha`) for this dataset. Justify your choice using at least two specific MSE
   values and one discussion of why regularization did or did not help here, given the
   correlation structure of the features.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

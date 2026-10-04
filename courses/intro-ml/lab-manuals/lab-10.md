# Lab Manual 10 — Cross-Validation & Hyperparameter Search

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Apply k-fold cross-validation, `GridSearchCV`, and learning curves to select a well-generalizing
model.

## Setup
Continue with scikit-learn; create `lab10.ipynb`.

## Procedure
1. **Task A — Cross-validation:** using `StratifiedKFold(n_splits=5)`, report the mean and
   standard deviation of cross-validated accuracy for a `RandomForestClassifier` on the provided
   dataset.
2. **Task B — Grid search:** run `GridSearchCV` over a small grid of `max_depth` and
   `n_estimators` values; report `best_params_` and `best_score_`.
3. **Task C — Learning curve:** plot a learning curve (training size vs. train/validation score)
   for the best model from Task B; state whether it shows signs of high bias, high variance, or
   neither.
4. **Task D — Nested CV (brief):** run a small nested cross-validation (3-fold inner, 5-fold
   outer) over the same grid as Task B, and compare the nested CV score to the (non-nested)
   `best_score_` from Task B.

## Expected Output
A notebook with Tasks A–D, including the learning curve plot.

## Submission
Submit `lab10.ipynb` by the end of the lab session.

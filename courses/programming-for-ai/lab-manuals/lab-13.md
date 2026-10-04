# Lab Manual 13 — Cross-Validation & Hyperparameter Tuning

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Apply k-fold cross-validation and `GridSearchCV`; diagnose overfitting via learning curves.

## Setup
Create `lab13.ipynb`; reuse a classifier/dataset from Lab 11.

## Procedure
1. **Task A — Cross-validation:** run `cross_val_score` (cv=5) for a chosen model; report mean
   and standard deviation of scores.
2. **Task B — Grid search:** define a hyperparameter grid for the model; run `GridSearchCV`;
   report the best parameters and best score.
3. **Task C — Learning curves:** plot training vs. validation error against training-set size
   for the tuned model; diagnose the fit regime (underfit/overfit/well-fit).
4. **Task D — Regularized comparison:** for a linear model, compare unregularized vs. L2
   (`Ridge`) regularized versions on the learning curve plot.

## Expected Output
A notebook with Tasks A–D.

## Submission
Submit `lab13.ipynb` by the end of the lab session.

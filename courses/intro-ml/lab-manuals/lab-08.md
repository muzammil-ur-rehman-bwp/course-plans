# Lab Manual 8 — Boosting

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Fit AdaBoost and Gradient Boosting classifiers and compare them to Week 7's bagging/Random Forest
results.

## Setup
Continue with scikit-learn; create `lab08.ipynb`. Reuse the Week 6–7 dataset.

## Procedure
1. **Task A — AdaBoost:** fit `AdaBoostClassifier` with depth-1 tree stumps, `n_estimators=200`;
   report train/test accuracy.
2. **Task B — Gradient Boosting:** fit `GradientBoostingClassifier` at `learning_rate` in
   `{0.01, 0.1, 0.5}` (fixed `n_estimators=200`); report test accuracy for each and identify the
   best.
3. **Task C — Overfitting check:** for the best Gradient Boosting configuration, plot test
   accuracy (or test loss, if available) against `n_estimators` from 10 to 500 to check whether
   accuracy degrades at very high `n_estimators` (overfitting).
4. **Task D — Comparison table:** build a single table comparing train/test accuracy for a single
   tree (Week 6), bagging, Random Forest (Week 7), AdaBoost, and Gradient Boosting (this week).

## Expected Output
A notebook with Tasks A–D, including the overfitting-check plot and the final comparison table.

## Submission
Submit `lab08.ipynb` by the end of the lab session.

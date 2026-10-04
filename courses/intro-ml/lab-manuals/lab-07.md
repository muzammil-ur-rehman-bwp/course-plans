# Lab Manual 7 — Bagging & Random Forests

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Compare a single decision tree, a bagging ensemble, and a Random Forest, and interpret feature
importances.

## Setup
Continue with scikit-learn; create `lab07.ipynb`. Reuse the Week 6 dataset for direct comparison.

## Procedure
1. **Task A — Baseline:** fit a single (unpruned) `DecisionTreeClassifier`; report train/test
   accuracy.
2. **Task B — Bagging:** fit a `BaggingClassifier` with 100 tree estimators; report train/test
   accuracy and compare the train/test gap to Task A.
3. **Task C — Random Forest:** fit a `RandomForestClassifier` with 200 trees and `oob_score=True`;
   report train/test accuracy and the OOB score.
4. **Task D — Feature importance:** plot the Random Forest's `feature_importances_`; identify the
   top 3 features and state one caveat about interpreting this ranking.

## Expected Output
A notebook with Tasks A–D, including the feature-importance bar plot.

## Submission
Submit `lab07.ipynb` by the end of the lab session.

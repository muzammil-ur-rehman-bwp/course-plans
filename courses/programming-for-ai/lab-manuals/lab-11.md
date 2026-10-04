# Lab Manual 11 — Classification & Evaluation Metrics

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Train multiple classifiers and evaluate them with appropriate metrics.

## Setup
Create `lab11.ipynb`; use a provided classification dataset.

## Procedure
1. **Task A — Train 3 classifiers:** fit `KNeighborsClassifier`, `DecisionTreeClassifier`, and
   `SVC` on the same train split.
2. **Task B — Confusion matrices:** compute and display a confusion matrix for each classifier.
3. **Task C — Metrics table:** build a table comparing accuracy, precision, recall, and F1 for
   all three classifiers.
4. **Task D — ROC/AUC:** plot ROC curves for all three (where `predict_proba` is available) and
   report AUC for each.
5. **Task E — Recommendation:** based on Tasks B–D, recommend one classifier for this dataset
   and justify using specific metric values.

## Expected Output
A notebook with Tasks A–E.

## Submission
Submit `lab11.ipynb` by the end of the lab session.

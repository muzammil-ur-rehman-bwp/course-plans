# Lab Manual 4 — Logistic Regression & Classification Metrics

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Fit a logistic regression classifier and evaluate it with a full suite of classification metrics,
including under class imbalance.

## Setup
Continue with scikit-learn; create `lab04.ipynb`.

## Procedure
1. **Task A — Fit:** fit `LogisticRegression` on the provided binary dataset; report accuracy.
2. **Task B — Full report:** compute the confusion matrix and `classification_report`
   (precision/recall/F1 per class).
3. **Task C — ROC/AUC:** plot the ROC curve and compute AUC using `predict_proba`.
4. **Task D — Imbalance:** on the provided imbalanced dataset, compare a default
   `LogisticRegression` to one fit with `class_weight="balanced"`; report recall for the minority
   class in both cases and explain the difference.

## Expected Output
A notebook with Tasks A–D, including the ROC plot and a short written comparison for Task D.

## Submission
Submit `lab04.ipynb` by the end of the lab session.

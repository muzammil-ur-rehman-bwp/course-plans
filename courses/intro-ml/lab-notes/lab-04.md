# Lab Notes 4 — Logistic Regression & Classification Metrics

**Concept recap:** logistic regression outputs a probability via the sigmoid; precision/recall/F1
and ROC/AUC give a fuller evaluation than accuracy, especially on imbalanced data.

**Common pitfalls:**
- Reporting only accuracy on an imbalanced dataset — a classifier that always predicts the
  majority class can score high accuracy while being useless for the minority class.
- Using `predict()` output (hard 0/1 labels) instead of `predict_proba()` when computing ROC/AUC —
  ROC/AUC require a score or probability, not the final thresholded prediction.
- Not stratifying the train/test split (`train_test_split(..., stratify=y)`) on an imbalanced
  dataset, which can produce a test set with a very different class ratio than training.

**Debugging tip:** if `roc_auc_score` raises an error or gives an unexpected value, check that
you passed probabilities/scores (`predict_proba(...)[:, 1]`), not the predicted class labels.

**Instructor tip:** have students compute recall for the minority class with and without
`stratify=y` in the split, across a few random seeds, to see how an unstratified split can swing
results on small imbalanced datasets.

# Lab Notes 11 — Classification & Evaluation

**Concept recap:** confusion matrix breaks predictions into TP/FP/TN/FN; precision/recall/F1
give a fuller picture than accuracy alone, especially on imbalanced classes; ROC/AUC evaluates
across all classification thresholds at once.

**Common pitfalls:**
- Comparing classifiers using accuracy alone when the dataset is imbalanced (a classifier that
  always predicts the majority class can have high accuracy and be useless).
- Calling `SVC(probability=True)` is required to get `predict_proba` for ROC curves (otherwise
  `SVC` only gives hard class labels) — easy to forget and get an error.
- Not setting a consistent `random_state` across classifiers, making comparisons noisy.

**Debugging tip:** always print the class distribution (`y.value_counts()`/`Counter(y)`) before
interpreting accuracy — it immediately flags whether accuracy alone is trustworthy here.

**Instructor tip:** pick (or construct) a deliberately imbalanced dataset variant for Task E so
the "accuracy is misleading" lesson is unavoidable, not just theoretical.

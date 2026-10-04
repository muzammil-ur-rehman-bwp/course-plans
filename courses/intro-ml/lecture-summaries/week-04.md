# Week 4 Summary — Logistic Regression & Classification Metrics in Depth

**Key takeaways:**
- Logistic regression applies the sigmoid function to a linear score to output a class
  probability; the default decision boundary is at `P = 0.5`, equivalent to a linear boundary in
  `z`.
- Precision, recall, and F1 give a fuller picture than accuracy, especially on imbalanced data;
  the right metric depends on the relative cost of false positives vs. false negatives.
- ROC/AUC evaluate ranking quality across all thresholds, not just one.
- One-vs-rest and one-vs-one extend binary classifiers to multi-class problems; class weighting
  and resampling address imbalance.

**You should now be able to:** fit and evaluate a logistic regression classifier with a full
classification report and ROC/AUC; choose an appropriate metric and imbalance strategy for a
given dataset.

**Next week:** k-Nearest Neighbors and Naive Bayes — lazy vs. probabilistic classification.

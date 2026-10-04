# Week 15 Summary — ML Systems in Practice

**Key takeaways:**
- A full scikit-learn `Pipeline` (preprocessing + model) is a single, leak-safe, tunable,
  deployable object; `joblib` saves and reloads a fitted pipeline for later inference.
- Deployment requires input validation, awareness of data/model drift, and reproducibility
  practices (fixed seeds, versioned schemas).
- Bias in training data (historical, sampling, measurement) propagates into trained models and
  is not fixable by algorithm choice alone.
- Fairness metrics (demographic parity, equal opportunity, equalized odds) can conflict with each
  other and with overall accuracy; responsible deployment requires documenting a model's known
  limitations and performance gaps across groups.

**You should now be able to:** build, persist, and reload a full ML pipeline; compute group-wise
performance metrics to audit a model for a fairness gap.

**Next week:** capstone project presentations and course review.

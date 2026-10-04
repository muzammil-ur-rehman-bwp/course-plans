# Lab Notes 7 — Bagging & Random Forests

**Concept recap:** bagging reduces variance by averaging models trained on bootstrap samples;
Random Forests further decorrelate trees via random feature subsets at each split.

**Common pitfalls:**
- Misinterpreting feature importance as "this feature causes the outcome" rather than "this
  feature was useful for these particular splits" — impurity-based importance is not a causal
  measure and can be biased toward high-cardinality numeric features.
- Expecting a Random Forest to always beat bagging by a large margin — on some datasets the gain
  is modest; the key is comparing the train/test gap, not just raw accuracy.
- Using too few estimators (`n_estimators`) for a stable OOB estimate or importance ranking —
  results can be noisy with very few trees.

**Debugging tip:** if OOB score and test accuracy disagree substantially, check that the test set
was not accidentally included in the bootstrap sampling process (it should never be — OOB uses
only the training set's own left-out samples).

**Instructor tip:** have students re-run the Random Forest fit with a different `random_state`
and compare the feature-importance ranking's top 3 features across runs, to see how stable (or
not) the ranking is.

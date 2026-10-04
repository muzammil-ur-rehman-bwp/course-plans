# Lab Notes 10 — Cross-Validation & Hyperparameter Search

**Concept recap:** cross-validation gives a more stable performance estimate than a single split;
nested CV avoids optimistic bias when the same data drives both tuning and final evaluation.

**Common pitfalls:**
- Reporting `GridSearchCV.best_score_` as if it were an unbiased estimate of real-world
  performance — it is itself a cross-validated score used to *select* hyperparameters, so it can
  be mildly optimistic; nested CV (Task D) exists precisely to correct for this.
- Running cross-validation on data that was scaled/imputed on the *full* dataset before splitting
  into folds — each fold's preprocessing must be refit within that fold (achieved automatically by
  passing a `Pipeline`, not a bare preprocessor, into `cross_val_score`/`GridSearchCV`).
- Using plain `KFold` on an imbalanced classification dataset instead of `StratifiedKFold`,
  producing folds with inconsistent class ratios.

**Debugging tip:** if cross-validated scores have unexpectedly high variance across folds, check
fold sizes and class balance — a small dataset with few folds can produce noisy per-fold
estimates even with correct code.

**Instructor tip:** have students pass a raw `StandardScaler` + model (not wrapped in a
`Pipeline`) into `cross_val_score` by mistake once, fit outside the loop, to see the subtly
leaked, overly optimistic score — then fix it with a `Pipeline` and compare.

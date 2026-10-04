# Lab Notes 13 — Cross-Validation & Tuning

**Concept recap:** k-fold CV averages performance across k train/validation splits for a more
robust estimate; `GridSearchCV` wraps CV with an exhaustive hyperparameter search; learning
curves reveal the bias/variance regime a model is in.

**Common pitfalls:**
- Running `GridSearchCV` with the test set instead of only the training set — the test set must
  remain untouched until the very final evaluation.
- Choosing a hyperparameter grid too narrow to actually find an improvement, or too wide to
  finish running in lab time — start with a small, sensible grid.
- Misreading a learning curve: a small, stable gap between curves is normal and not automatically
  "overfitting" — focus on whether validation error is still improving with more data.

**Debugging tip:** if `GridSearchCV` takes too long, reduce `cv` folds or the grid size for the
in-lab exercise, then mention that production tuning would use more compute/time.

**Instructor tip:** Task D's regularization comparison is most convincing on a dataset with many
features relative to samples — pick/construct the dataset accordingly.

# Week 10 Summary — Model Evaluation & Selection in Depth

**Key takeaways:**
- k-fold cross-validation gives a more stable performance estimate than a single train/test
  split; `StratifiedKFold` preserves class proportions across folds.
- Nested cross-validation separates hyperparameter tuning from final performance estimation,
  avoiding optimistic bias from reusing the same data for both.
- `GridSearchCV` and `RandomizedSearchCV` automate hyperparameter search; the latter scales
  better to large or continuous search spaces.
- Learning curves diagnose underfitting (both curves low and converged) vs. overfitting (a
  persistent train/validation gap); the bias-variance decomposition explains why.

**You should now be able to:** run k-fold and nested cross-validation; use `GridSearchCV`/
`RandomizedSearchCV`; plot and interpret a learning curve to choose a remedy for under/overfitting.

**Reminder:** Assignment 3 assigned this week; Capstone project proposal due this week.
**Next week:** feature engineering and preprocessing.

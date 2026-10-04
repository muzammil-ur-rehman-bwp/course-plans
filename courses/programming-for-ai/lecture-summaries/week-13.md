# Week 13 Summary — Model Evaluation, Overfitting, Hyperparameter Tuning

**Key takeaways:**
- The bias-variance tradeoff explains underfitting (high bias) vs. overfitting (high variance).
- k-fold cross-validation gives a more robust performance estimate than a single split.
- `GridSearchCV`/`RandomizedSearchCV` automate hyperparameter tuning.
- L1/L2 regularization penalizes large weights to reduce overfitting.
- Learning curves (training vs. validation error vs. dataset size) diagnose which fit regime a
  model is in.

**You should now be able to:** cross-validate a model; tune hyperparameters systematically;
read a learning curve to diagnose overfitting/underfitting.

**Reminder:** Assignment 4 (model tuning/evaluation) is assigned this week, due start of Week 15.
**Next week:** neural networks — the perceptron and the forward pass.

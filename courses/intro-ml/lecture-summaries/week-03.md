# Week 3 Summary — Regularized Regression

**Key takeaways:**
- Regularization adds a penalty on coefficient magnitude to reduce variance (overfitting), at the
  cost of a small increase in bias.
- Ridge (L2) shrinks coefficients smoothly toward zero; Lasso (L1) can drive coefficients to
  exactly zero, performing feature selection; Elastic Net combines both penalties.
- The regularization strength `alpha` is a hyperparameter chosen by comparing validation
  performance across a range of values.
- Regularized models require feature scaling (fit on training data only) to penalize coefficients
  fairly across features of different scales.

**You should now be able to:** fit Ridge, Lasso, and Elastic Net models in scikit-learn inside a
correctly scaled pipeline; explain the difference between L1 and L2 penalties and when to prefer
each.

**Reminder:** Assignment 1 (regression & regularization) was assigned this week.
**Next week:** logistic regression and classification metrics in depth.

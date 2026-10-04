# Week 10 Summary — Regression with scikit-learn

**Key takeaways:**
- Linear regression minimizes MSE; scikit-learn solves it via least squares (gradient descent is
  the general-purpose alternative used later for neural networks).
- Logistic regression uses the sigmoid function to output a probability for binary
  classification; the 0.5 threshold defines the decision boundary.
- Feature scaling (`StandardScaler`) should be fit on training data only, then applied to both
  train and test, to avoid data leakage.
- Model coefficients are interpretable (change in prediction per unit change in a feature).

**You should now be able to:** fit and evaluate linear/logistic regression models in
scikit-learn; correctly apply feature scaling without leakage.

**Reminder:** Capstone project proposal is due this week.
**Next week:** classification with k-NN/Decision Trees/SVM, and formal evaluation metrics.

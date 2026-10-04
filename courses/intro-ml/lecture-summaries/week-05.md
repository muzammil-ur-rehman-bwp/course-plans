# Week 5 Summary — k-Nearest Neighbors and Naive Bayes

**Key takeaways:**
- k-NN is a lazy, distance-based classifier with no training phase; it requires feature scaling
  and is sensitive to the choice of `k` (bias-variance tradeoff) and to the curse of
  dimensionality.
- Naive Bayes applies Bayes' rule with a conditional-independence assumption between features
  given the class; Gaussian and Multinomial variants suit continuous and count features
  respectively.
- Probabilities in Naive Bayes are computed in log-space for numerical stability.
- k-NN, Naive Bayes, and logistic regression produce visibly different decision-boundary shapes
  on the same data.

**You should now be able to:** fit and tune k-NN; fit Gaussian/Multinomial Naive Bayes; explain
when each model's assumptions are likely to hold or break down.

**Next week:** decision trees — entropy, information gain, Gini impurity, and pruning.

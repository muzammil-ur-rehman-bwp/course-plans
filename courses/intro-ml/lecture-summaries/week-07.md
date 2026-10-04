# Week 7 Summary — Ensemble Methods I — Bagging & Random Forests

**Key takeaways:**
- Bagging trains many models on bootstrap samples and averages/votes their predictions, reducing
  variance; the benefit is limited by correlation between the ensemble's models.
- Random Forests add random feature subsets at each split to further decorrelate trees, reducing
  variance beyond plain bagging.
- Out-of-bag error gives a free validation estimate from each tree's unused bootstrap samples.
- Feature importance (mean impurity decrease) is useful but has known biases and is not a causal
  measure.

**You should now be able to:** fit bagging ensembles and Random Forests in scikit-learn; extract
and interpret (with appropriate caveats) feature importances; explain why Random Forests
generalize better than a single decision tree.

**Next week:** ensemble methods II — boosting (AdaBoost, Gradient Boosting), and midterm review.

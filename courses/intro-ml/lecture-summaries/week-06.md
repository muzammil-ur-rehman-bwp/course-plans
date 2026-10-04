# Week 6 Summary — Decision Trees

**Key takeaways:**
- Entropy and Gini impurity both measure node impurity; trees are grown by greedily picking the
  split that maximizes information gain (entropy reduction) or Gini reduction at each node.
- A fully-grown decision tree tends to overfit by memorizing training data.
- Pruning controls (`max_depth`, `min_samples_leaf`, cost-complexity pruning via `ccp_alpha`)
  trade a small increase in training error for better generalization.
- Decision trees are naturally interpretable and can be visualized directly.

**You should now be able to:** compute entropy, information gain, and Gini impurity by hand;
fit, visualize, and prune a decision tree in scikit-learn; diagnose overfitting from a
train/test accuracy gap.

**Next week:** ensemble methods I — bagging and Random Forests.

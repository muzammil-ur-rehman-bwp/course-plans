# Lab Notes 6 — Decision Trees

**Concept recap:** trees split greedily to maximize information gain/Gini reduction; unpruned
trees overfit, and pruning controls trade training accuracy for generalization.

**Common pitfalls:**
- Concluding a tree is "better" purely from training accuracy — an unpruned tree can reach ~100%
  training accuracy while performing worse than a pruned tree on test data.
- Setting `max_depth` far too small and misreading the resulting underfitting as "the model
  doesn't work," rather than as a pruning-strength issue.
- Forgetting that `plot_tree` on a deep, unpruned tree produces an unreadable plot — use a depth
  limit for visualization purposes even if the fitted model itself is unpruned.

**Debugging tip:** if `cost_complexity_pruning_path` returns many candidate `ccp_alpha` values,
plot test accuracy against `ccp_alpha` (log scale if the range is wide) rather than guessing a
single value.

**Instructor tip:** have students report both the tree's depth and leaf count for each
`max_depth` setting — the leaf count makes model complexity concrete alongside the accuracy
numbers.

# Week 6 — Lecture Content: Decision Trees

## 1. Entropy
Entropy measures the impurity (disorder) of a set of labels. For a binary target with class
proportions `p` and `1-p`:
```
H(p) = - p * log2(p) - (1-p) * log2(1-p)
```
General form for `K` classes with proportions `p_1, ..., p_K`:
```
H = - sum_{k=1}^{K} p_k * log2(p_k)
```
`H = 0` when a node is pure (one class only); `H` is maximized when classes are evenly mixed
(`H = 1` for a balanced binary node).

## 2. Information Gain
A split is evaluated by how much it reduces entropy, weighted by the resulting subset sizes:
```
IG(split) = H(parent) - sum_{child} (n_child / n_parent) * H(child)
```
The tree-growing algorithm picks, at each node, the feature and threshold that maximize
information gain (ID3 for categorical features; CART generalizes this to numeric thresholds).

**Worked example:** a node with 10 examples (6 positive, 4 negative) has
`H = -0.6*log2(0.6) - 0.4*log2(0.4) ≈ 0.971`. A split into a pure 4-positive leaf and a 6-example
leaf (2 positive, 4 negative, `H ≈ 0.918`) gives
`IG = 0.971 - (4/10 * 0 + 6/10 * 0.918) ≈ 0.420`.

## 3. Gini Impurity
CART (scikit-learn's default) typically uses **Gini impurity** instead of entropy:
```
Gini = 1 - sum_{k=1}^{K} p_k^2
```
Gini and entropy usually select similar splits in practice; Gini is slightly cheaper to compute
(no logarithm).

## 4. Tree Construction (ID3/CART, Conceptually)
Starting at the root with all training data, the algorithm:
1. Evaluates every candidate feature/threshold split by information gain (or Gini reduction).
2. Chooses the split that maximizes the gain.
3. Recurses on each resulting child node.
4. Stops when a node is pure, a maximum depth is reached, or further splitting would not
   sufficiently reduce impurity (or violates a minimum-samples constraint).

```python
from sklearn.tree import DecisionTreeClassifier, plot_tree
import matplotlib.pyplot as plt

tree = DecisionTreeClassifier(criterion="gini", random_state=42)
tree.fit(X_train, y_train)

plt.figure(figsize=(14, 8))
plot_tree(tree, feature_names=feature_names, class_names=class_names, filled=True)
plt.show()
```

## 5. Overfitting and Pruning
A fully-grown tree (no depth limit) can create a leaf for every training example, achieving 100%
training accuracy while generalizing poorly — the classic high-variance failure mode.
**Pruning** controls complexity:
- `max_depth`: caps how deep the tree can grow.
- `min_samples_leaf` / `min_samples_split`: requires a minimum number of examples before
  splitting/forming a leaf.
- **Cost-complexity pruning** (`ccp_alpha`): grows the full tree, then prunes back branches whose
  removal does not increase impurity by more than `ccp_alpha`, trading a small increase in
  training impurity for a simpler, less variance-prone tree.

```python
# Compare an unpruned tree to a depth-limited one
unpruned = DecisionTreeClassifier(random_state=42).fit(X_train, y_train)
pruned = DecisionTreeClassifier(max_depth=4, min_samples_leaf=5, random_state=42).fit(X_train, y_train)

print("Unpruned — train:", unpruned.score(X_train, y_train), "test:", unpruned.score(X_test, y_test))
print("Pruned   — train:", pruned.score(X_train, y_train), "test:", pruned.score(X_test, y_test))
```
A large train/test accuracy gap for the unpruned tree, closed substantially by the pruned tree,
directly illustrates the bias-variance tradeoff for this model family.

## 6. In-Class Exercise
By hand, compute the entropy of a 10-example node (7 positive, 3 negative), then compute the
information gain of a given candidate split.

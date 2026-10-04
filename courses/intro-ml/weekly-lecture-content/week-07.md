# Week 7 — Lecture Content: Ensemble Methods I — Bagging & Random Forests

## 1. Bootstrap Aggregating (Bagging)
**Bagging** trains many copies of a base model (commonly a decision tree) on different
**bootstrap samples** — samples of size `n` drawn with replacement from the training set of size
`n` — and combines their predictions by majority vote (classification) or averaging
(regression). Each bootstrap sample omits roughly 1/3 of the original examples on average and
duplicates others.

```python
from sklearn.ensemble import BaggingClassifier
from sklearn.tree import DecisionTreeClassifier

bagged = BaggingClassifier(
    estimator=DecisionTreeClassifier(),
    n_estimators=100,
    random_state=42,
)
bagged.fit(X_train, y_train)
```

## 2. Why Bagging Reduces Variance
Decision trees are high-variance: small changes in training data can produce quite different
trees. If `T` models each have variance `sigma^2` and are pairwise correlated with correlation
`rho`, the variance of their average is:
```
Var(average) = rho * sigma^2 + (1 - rho) * sigma^2 / T
```
As `T` grows, the second term shrinks toward zero, but the first term (driven by correlation
between models) does not — so **reducing correlation between the ensemble's models**, not just
adding more of them, is key to further variance reduction. This motivates Random Forests.

## 3. Random Forests
A **Random Forest** bags decision trees but adds a second source of randomness: at each split,
only a random subset of features (typically `sqrt(p)` for classification) is considered as
candidates. This decorrelates the trees — different trees tend to pick different features at the
top of the tree — which reduces `rho` and therefore the ensemble's variance beyond plain bagging.

```python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(n_estimators=200, max_features="sqrt", random_state=42)
rf.fit(X_train, y_train)
print("Train:", rf.score(X_train, y_train), "Test:", rf.score(X_test, y_test))
```

## 4. Out-of-Bag (OOB) Error
Each bootstrap sample leaves out roughly a third of the data; each tree's left-out examples form
a free validation set for that tree. Averaging each example's prediction over only the trees that
did not see it in training gives the **out-of-bag error estimate**, a convenient validation
score obtained without a separate held-out set.
```python
rf_oob = RandomForestClassifier(n_estimators=200, oob_score=True, random_state=42)
rf_oob.fit(X_train, y_train)
print("OOB score:", rf_oob.oob_score_)
```

## 5. Feature Importance
Random Forests report feature importance as the mean decrease in impurity (Gini or entropy)
attributable to each feature, averaged across all trees:
```python
import pandas as pd
import matplotlib.pyplot as plt

importances = pd.Series(rf.feature_importances_, index=feature_names).sort_values(ascending=False)
importances.plot.bar()
plt.ylabel("Mean impurity decrease"); plt.title("Random Forest feature importances")
plt.show()
```
**Caveat:** impurity-based importance can be biased toward high-cardinality numeric features
(those with many possible split points) and does not by itself establish a causal relationship —
it should be read as "how useful this feature was for these particular splits," not as proof the
feature is the true driver of the outcome. Permutation importance (`sklearn.inspection.
permutation_importance`) is a more robust, model-agnostic alternative when importance rankings
matter a great deal.

## 6. In-Class Exercise
Fit a single decision tree, a `BaggingClassifier`, and a `RandomForestClassifier` on the same
dataset; compare train/test accuracy for all three and discuss which shows the smallest
train/test gap.

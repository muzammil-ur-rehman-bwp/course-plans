# Week 5 — Lecture Content: k-Nearest Neighbors and Naive Bayes

## 1. k-Nearest Neighbors: Lazy Learning
k-NN has no explicit training phase — it simply stores the training data (**lazy learning**). To
classify a new point `x`, it finds the `k` closest training points (by a distance metric, usually
Euclidean) and predicts the majority class among them (or the mean, for regression).
```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline

knn = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=5))
knn.fit(X_train, y_train)
```
**Feature scaling matters a great deal for k-NN:** since it is purely distance-based, a feature
on a much larger numeric scale would dominate the distance calculation.

## 2. Choosing k
Small `k` (e.g., `k=1`) gives a highly flexible, low-bias/high-variance decision boundary
(sensitive to noise, risk of overfitting). Large `k` smooths the boundary (higher bias, lower
variance) and can underfit if too large. `k` is chosen by comparing validation accuracy across a
range of values (formalized with cross-validation in Week 10):
```python
import numpy as np
from sklearn.model_selection import cross_val_score

scores = []
for k in range(1, 31, 2):
    model = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=k))
    scores.append(cross_val_score(model, X_train, y_train, cv=5).mean())
best_k = range(1, 31, 2)[int(np.argmax(scores))]
```

## 3. The Curse of Dimensionality (Brief)
As the number of features grows, data points become increasingly sparse and roughly equidistant
from one another in high-dimensional space, so the notion of a "nearest" neighbor becomes less
meaningful. k-NN (and other distance-based methods) tend to degrade in very high-dimensional
spaces unless dimensionality is first reduced (Week 13) or features are carefully selected
(Week 11).

## 4. Bayes' Rule Recap
```
P(y | x) = P(x | y) * P(y) / P(x)
```
`P(y)` is the **prior** probability of class `y`; `P(x|y)` is the **likelihood** of observing
features `x` given class `y`; `P(y|x)` is the **posterior** we want to predict with. `P(x)` is a
normalizing constant, often skipped since we only need the class that maximizes the numerator.

## 5. Naive Bayes
Naive Bayes assumes features are **conditionally independent given the class** — "naive" because
this is rarely exactly true, yet the classifier often performs well regardless:
```
P(y | x_1, ..., x_p) ∝ P(y) * prod_{j=1}^{p} P(x_j | y)
```
- **GaussianNB:** assumes each feature, within each class, follows a Gaussian distribution — used
  for continuous features.
- **MultinomialNB:** models feature counts (e.g., word counts) as draws from a multinomial
  distribution — standard for text classification.

```python
from sklearn.naive_bayes import GaussianNB, MultinomialNB

gnb = GaussianNB()
gnb.fit(X_train, y_train)          # continuous features

mnb = MultinomialNB()
mnb.fit(X_train_counts, y_train)   # non-negative count features, e.g. word counts
```
In practice, probabilities are computed in **log-space** to avoid numerical underflow from
multiplying many small probabilities together:
```
log P(y|x) ∝ log P(y) + sum_j log P(x_j | y)
```

## 6. Comparing Decision Boundaries
k-NN produces flexible, often jagged boundaries that adapt to local data density; Naive Bayes
produces smoother boundaries shaped by its distributional assumptions; logistic regression (Week
4) produces a strictly linear boundary. Plotting all three on the same 2D dataset makes these
differences concrete.

```python
# Pattern for plotting a decision boundary over a 2D feature grid
import numpy as np
import matplotlib.pyplot as plt

xx, yy = np.meshgrid(np.linspace(x_min, x_max, 300), np.linspace(y_min, y_max, 300))
Z = model.predict(np.c_[xx.ravel(), yy.ravel()]).reshape(xx.shape)
plt.contourf(xx, yy, Z, alpha=0.3)
plt.scatter(X[:, 0], X[:, 1], c=y, edgecolor="k")
plt.show()
```

## 7. In-Class Exercise
Fit k-NN at `k=1` and `k=25` on the same dataset; plot both decision boundaries and discuss which
is more likely to overfit.

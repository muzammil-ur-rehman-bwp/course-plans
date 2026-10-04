# Week 9 — Lecture Content: Midterm Exam + Support Vector Machines

## 1. Midterm Exam
Covers Weeks 1–8: the ML workflow and bias-variance tradeoff; linear and regularized regression;
logistic regression and classification metrics; k-NN and Naive Bayes; decision trees; bagging,
Random Forests, and boosting. See the Week 8 review session notes for the consolidated topic
list.

## 2. Margin Maximization
A Support Vector Machine (SVM) for a linearly separable binary dataset finds the hyperplane
`w^T x + b = 0` that separates the two classes with the **largest possible margin** — the
distance from the hyperplane to the nearest point of either class. Points on the boundary of the
margin are the **support vectors**; they alone determine the decision boundary (points further
away could be moved or removed without changing it).
```
Maximize margin  <=>  minimize (1/2) * ||w||^2
subject to  y_i * (w^T x_i + b) >= 1   for all training examples i
```
A larger margin tends to generalize better, as it gives new points more "room" before crossing
the boundary.

## 3. Soft Margins
Real data is rarely perfectly separable. The **soft-margin** formulation allows some points to
violate the margin (or even be misclassified), introducing slack variables `xi_i >= 0` and a
penalty hyperparameter `C`:
```
Minimize  (1/2)*||w||^2 + C * sum_i xi_i
subject to  y_i*(w^T x_i + b) >= 1 - xi_i,   xi_i >= 0
```
- **Large `C`:** heavily penalizes margin violations — a narrower margin that fits training data
  more closely (lower bias, higher variance risk).
- **Small `C`:** tolerates more violations for a wider margin (higher bias, lower variance).

```python
from sklearn.svm import SVC
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline

svm_linear = make_pipeline(StandardScaler(), SVC(kernel="linear", C=1.0))
svm_linear.fit(X_train, y_train)
```

## 4. The Kernel Trick
Many datasets are not linearly separable in their original feature space but become separable
after mapping to a higher-dimensional space. Computing that mapping explicitly can be expensive
or infinite-dimensional; the **kernel trick** computes the inner product of mapped points
directly via a **kernel function** `K(x_i, x_j)`, without ever materializing the mapping:
- **Linear kernel:** `K(x_i, x_j) = x_i^T x_j` — equivalent to no mapping; use when data is (close
  to) linearly separable.
- **Polynomial kernel:** `K(x_i, x_j) = (gamma * x_i^T x_j + r)^d` — models polynomial decision
  boundaries of degree `d`.
- **RBF (Gaussian) kernel:** `K(x_i, x_j) = exp(-gamma * ||x_i - x_j||^2)` — maps to an
  infinite-dimensional space; a very flexible default choice for non-linear data, with `gamma`
  controlling how far each point's influence reaches (large `gamma`: tight, locally-focused
  boundary and risk of overfitting; small `gamma`: smoother, more global boundary).

```python
svm_poly = make_pipeline(StandardScaler(), SVC(kernel="poly", degree=3, C=1.0))
svm_rbf = make_pipeline(StandardScaler(), SVC(kernel="rbf", C=1.0, gamma="scale"))

for name, model in [("linear", svm_linear), ("poly", svm_poly), ("rbf", svm_rbf)]:
    model.fit(X_train, y_train)
    print(name, "test accuracy:", model.score(X_test, y_test))
```
**Feature scaling is essential for SVMs:** the margin and all kernel computations depend directly
on distances/inner products between feature vectors, so unscaled features distort the geometry.

## 5. Choosing C and Gamma
`C` and `gamma` (for the RBF/polynomial kernels) are hyperparameters, usually chosen together via
a grid search with cross-validation (Week 10) rather than individually, since they interact: a
small `gamma` with a large `C` can behave similarly to a larger `gamma` with a smaller `C`.

## 6. In-Class Exercise
On a small, linearly separable 2D dataset plotted on the board/slide, mark by eye which points
are likely to be support vectors for the maximum-margin hyperplane, then verify with `SVC` and
`support_vectors_`.

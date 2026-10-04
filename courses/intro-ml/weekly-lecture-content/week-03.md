# Week 3 — Lecture Content: Regularized Regression

## 1. Why Regularize?
Ordinary least squares minimizes training error with no penalty on coefficient size. With many
features, few examples, or strongly correlated features, this can produce very large coefficients
that fit training noise — high variance, poor generalization. **Regularization** adds a penalty
on coefficient magnitude to the cost function, trading a small increase in bias for a reduction in
variance, often improving test performance.

## 2. Ridge Regression (L2)
Ridge adds the squared L2 norm of the coefficients to the MSE cost:
```
J_ridge(theta) = (1/n) * ||X theta - y||^2 + alpha * sum_{j=1}^{p} theta_j^2
```
(The intercept is typically excluded from the penalty.) Larger `alpha` shrinks coefficients
toward zero but, for ordinary Ridge, rarely to exactly zero — all features are retained, with
reduced influence.

```python
from sklearn.linear_model import Ridge
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline

ridge = make_pipeline(StandardScaler(), Ridge(alpha=1.0))
ridge.fit(X_train, y_train)
```

## 3. Lasso Regression (L1)
Lasso adds the L1 norm instead:
```
J_lasso(theta) = (1/n) * ||X theta - y||^2 + alpha * sum_{j=1}^{p} |theta_j|
```
The L1 penalty's geometry (a diamond-shaped constraint region vs. Ridge's circular one) tends to
push some coefficients to **exactly zero**, performing automatic feature selection — useful when
many features are expected to be irrelevant.

```python
from sklearn.linear_model import Lasso

lasso = make_pipeline(StandardScaler(), Lasso(alpha=0.1))
lasso.fit(X_train, y_train)
print("Number of zeroed coefficients:", (lasso[-1].coef_ == 0).sum())
```

## 4. Elastic Net
Elastic Net combines both penalties with a mixing parameter `l1_ratio in [0, 1]`:
```
J_enet(theta) = (1/n) * ||X theta - y||^2
                + alpha * ( l1_ratio * sum |theta_j| + (1 - l1_ratio)/2 * sum theta_j^2 )
```
`l1_ratio = 1` reduces to Lasso, `l1_ratio = 0` to Ridge. Elastic Net is useful when features are
correlated — Lasso tends to arbitrarily pick one of a correlated group, while Elastic Net's L2
component encourages correlated features to be kept or shrunk together.

```python
from sklearn.linear_model import ElasticNet

enet = make_pipeline(StandardScaler(), ElasticNet(alpha=0.1, l1_ratio=0.5))
enet.fit(X_train, y_train)
```

## 5. Choosing the Regularization Strength
`alpha` is a hyperparameter: `alpha = 0` recovers ordinary least squares; very large `alpha`
shrinks all coefficients toward zero (underfitting). The right value is chosen by comparing
validation performance across a range of `alpha` values (formalized with cross-validation in
Week 10); scikit-learn also offers `RidgeCV` and `LassoCV` for built-in cross-validated selection.

```python
import numpy as np
import matplotlib.pyplot as plt

alphas = np.logspace(-3, 3, 13)
coef_paths = []
for a in alphas:
    model = make_pipeline(StandardScaler(), Ridge(alpha=a)).fit(X_train, y_train)
    coef_paths.append(model[-1].coef_)

plt.plot(alphas, coef_paths)
plt.xscale("log")
plt.xlabel("alpha"); plt.ylabel("coefficient value")
plt.title("Ridge coefficient paths as regularization strength increases")
plt.show()
```

## 6. Feature Scaling Is Required
Regularization penalizes coefficient *magnitude*, which is meaningless unless features share a
common scale — an unscaled feature measured in the thousands would need a tiny coefficient
compared to one measured in single digits, and the penalty would unfairly shrink the latter more.
**Always** fit `StandardScaler` on the training data only, inside a `Pipeline`, before applying
Ridge/Lasso/Elastic Net.

## 7. In-Class Exercise
Fit Ridge and Lasso at `alpha` in `{0.01, 1, 100}` on a dataset with several correlated features;
report which coefficients shrink smoothly (Ridge) and which are driven to exactly zero (Lasso).

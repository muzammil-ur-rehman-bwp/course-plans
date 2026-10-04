# Week 2 — Lecture Content: Linear Regression in Depth

## 1. Simple and Multiple Linear Regression
Simple linear regression models a single feature: `y = w*x + b`. Multiple linear regression
generalizes to `p` features:
```
y_hat = w1*x1 + w2*x2 + ... + wp*xp + b = w^T x + b
```
In matrix form, stacking `n` examples as rows of `X` (with an added column of 1s for the
intercept) and weights as a vector `theta`: `y_hat = X @ theta`.

## 2. The Cost Function
Linear regression fits `theta` by minimizing the **Mean Squared Error**:
```
J(theta) = (1/n) * sum_{i=1}^{n} (y_hat_i - y_i)^2  = (1/n) * ||X @ theta - y||^2
```
MSE penalizes large errors quadratically and is differentiable everywhere, which is what makes
gradient-based optimization possible.

## 3. Gradient Descent — Derivation
The gradient of `J` with respect to `theta` is:
```
grad J(theta) = (2/n) * X^T @ (X @ theta - y)
```
**Batch gradient descent** repeatedly steps opposite the gradient, scaled by a learning rate
`alpha`:
```
theta := theta - alpha * grad J(theta)
```

```python
import numpy as np

def fit_gradient_descent(X, y, alpha=0.01, n_iters=1000):
    n, p = X.shape
    X_b = np.hstack([np.ones((n, 1)), X])     # add intercept column
    theta = np.zeros(p + 1)
    for _ in range(n_iters):
        grad = (2 / n) * X_b.T @ (X_b @ theta - y)
        theta -= alpha * grad
    return theta                               # theta[0] = intercept, theta[1:] = coefficients
```
Too large a learning rate can cause divergence (the cost increases); too small makes convergence
slow. Feature scaling (Week 3) makes gradient descent converge faster and more reliably when
features are on very different scales.

## 4. The Normal Equation
Setting `grad J(theta) = 0` and solving analytically gives the closed-form least-squares solution:
```
theta = (X^T X)^{-1} X^T y
```
This requires no learning rate and no iteration, but involves inverting a `(p+1) x (p+1)` matrix,
which becomes expensive for very large `p` (gradient descent, or its stochastic/mini-batch
variants, scale better). scikit-learn's `LinearRegression` uses an efficient least-squares solver
equivalent to the normal equation.

```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print("MSE:", mean_squared_error(y_test, y_pred))
print("R^2:", r2_score(y_test, y_pred))
print("Coefficients:", model.coef_, "Intercept:", model.intercept_)
```

## 5. Assumptions of Linear Regression
- **Linearity:** the true relationship between features and target is (approximately) linear.
- **Independence:** residuals (errors) are independent of each other.
- **Homoscedasticity:** residuals have roughly constant variance across the range of predictions.
- **Normality of residuals:** residuals are approximately normally distributed (mainly matters for
  valid confidence intervals/hypothesis tests on coefficients, less for prediction accuracy
  alone).

```python
import matplotlib.pyplot as plt

residuals = y_test - y_pred
plt.scatter(y_pred, residuals)
plt.axhline(0, color="red", linestyle="--")
plt.xlabel("Predicted value"); plt.ylabel("Residual")
plt.title("Residual plot — look for patterns (violations) vs. random scatter (OK)")
plt.show()
```
A residual plot with a clear curve or funnel shape (increasing spread) signals a violated
assumption — a sign to consider a non-linear model, a transformation, or regularization (Week 3).

## 6. In-Class Exercise
By hand or in code, run one iteration of batch gradient descent for a model with two parameters
on a 4-point dataset, and compare the resulting `theta` update direction with the normal
equation's exact solution.

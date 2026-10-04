# Week 10 — Lecture Content: Regression with scikit-learn

## 1. Linear Regression
Models the target as a linear combination of features: `y = w1*x1 + w2*x2 + ... + b`. Fit by
minimizing the **Mean Squared Error (MSE)** cost function:
```
MSE = (1/n) * sum((y_pred - y_true)^2)
```
**Gradient descent** (conceptual): iteratively adjust weights in the direction that reduces MSE
most steeply, scaled by a learning rate. scikit-learn's `LinearRegression` solves this
analytically (normal equation / least squares) rather than via gradient descent, but the cost
function being minimized is the same.

```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
mse = mean_squared_error(y_test, y_pred)
print(model.coef_, model.intercept_)
```

## 2. Logistic Regression
Despite the name, used for **binary classification**. Applies the sigmoid function to a linear
combination of features to output a probability in (0, 1):
```
P(y=1 | x) = 1 / (1 + exp(-(w·x + b)))
```
```python
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

clf = LogisticRegression()
clf.fit(X_train, y_train)
y_pred = clf.predict(X_test)
print(accuracy_score(y_test, y_pred))
```
A decision boundary at `P(y=1|x) = 0.5` separates the two predicted classes.

## 3. Feature Scaling
Models based on distances/gradients (including logistic regression) often perform better and
train faster when features are on comparable scales.
```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)  # use the SAME scaler fit on training data
```
**Important:** fit the scaler only on training data, then transform both train and test — fitting
on test data leaks information from the test set into preprocessing.

## 4. Interpreting Coefficients
For linear regression, `coef_[i]` is the change in predicted `y` for a one-unit increase in
feature `i`, holding other features fixed (only meaningful after scaling/encoding is consistent).

## 5. In-Class Exercise
Fit a `LinearRegression` model on a provided dataset, plot predicted vs. actual values, and fit a
`LogisticRegression` model on a binary dataset, reporting test accuracy.

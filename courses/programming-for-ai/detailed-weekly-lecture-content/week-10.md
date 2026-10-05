# Week 10: Regression with scikit-learn

## Learning Objectives

By the end of this lecture, you should be able to:

1. Describe linear regression and the mean squared error cost function.
2. Explain gradient descent and implement it for a simple linear model.
3. Train and evaluate `LinearRegression` and `LogisticRegression` in scikit-learn.
4. Scale features correctly, fitting the scaler on training data only.
5. Interpret model coefficients, and know when interpretation is safe.

## 1. Linear Regression

Linear regression models the target as a weighted sum of the features plus a constant.

```
y = w1*x1 + w2*x2 + ... + wp*xp + b
```

The weights `w` and the intercept `b` are what we learn. With one feature this is a straight line, with two features a plane, and with more features a hyperplane.

How do we decide which weights are best? We need a cost function that measures how wrong a set of weights is. The usual choice is the mean squared error.

```
MSE = (1/n) * sum((y_pred - y_true)^2)
```

Squaring has two effects. Errors in either direction count the same, and big errors count much more than small ones. A prediction that is off by 10 costs a hundred times as much as one off by 1. Training means finding the weights with the smallest MSE.

### 1.1 Gradient descent

Imagine standing on a hillside in fog, wanting to reach the valley. You cannot see the valley, but you can feel the slope under your feet. A sensible strategy is to take a step downhill, and repeat. Gradient descent does exactly this on the cost surface. The gradient of the cost with respect to the weights points uphill, so we move in the opposite direction, scaled by a learning rate `alpha`.

```
w := w - alpha * dCost/dw
```

For linear regression with MSE, the gradients are simple. The code below fits a line to noisy data by hand, so you can see the method working. It uses only NumPy.

```python
import numpy as np

rng = np.random.default_rng(0)
x = rng.uniform(0, 10, size=100)
y = 2.5 * x + 4.0 + rng.normal(0, 2.0, size=100)

w, b = 0.0, 0.0
alpha = 0.01
history = []

for step in range(2000):
    y_pred = w * x + b
    error = y_pred - y
    cost = np.mean(error ** 2)
    grad_w = 2 * np.mean(error * x)
    grad_b = 2 * np.mean(error)
    w -= alpha * grad_w
    b -= alpha * grad_b
    history.append(cost)

print(f"learned w = {w:.3f}, b = {b:.3f}  (true values 2.5 and 4.0)")
print(f"cost at step 0: {history[0]:.2f}, at the end: {history[-1]:.2f}")
```

The learned values should be close to the true 2.5 and 4.0, and not exactly equal, because the data is noisy. Try changing `alpha`. If it is too small, the cost falls very slowly. If it is too large, for instance 0.05 on this data, the steps overshoot the valley and the cost explodes.

```python
def run(alpha, steps=50):
    w, b = 0.0, 0.0
    for _ in range(steps):
        error = (w * x + b) - y
        w -= alpha * 2 * np.mean(error * x)
        b -= alpha * 2 * np.mean(error)
    return np.mean(((w * x + b) - y) ** 2)

for a in [0.0001, 0.001, 0.01, 0.05]:
    print(f"alpha={a:<7} cost after 50 steps = {run(a):.3g}")
```

The largest rate here makes the cost grow to a huge number. Choosing a learning rate is one of the first practical skills in training models, and we meet it again in the deep learning weeks.

scikit-learn's `LinearRegression` does not use gradient descent. Because the cost is a smooth bowl with one minimum, the best weights can be found directly by solving the normal equations, a linear algebra problem like the one in Week 3. The cost function being minimized is the same.

### 1.2 Using scikit-learn

```python
from sklearn.datasets import load_diabetes
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

data = load_diabetes()
X_train, X_test, y_train, y_test = train_test_split(
    data.data, data.target, test_size=0.2, random_state=42
)

model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

mse = mean_squared_error(y_test, y_pred)
print("MSE: ", round(mse, 1))
print("RMSE:", round(mse ** 0.5, 1))
print("R^2: ", round(r2_score(y_test, y_pred), 3))
print("intercept:", round(model.intercept_, 1))
for name, c in zip(data.feature_names, model.coef_):
    print(f"{name:>4s} {c:9.1f}")
```

Three numbers are worth reading. The MSE is in squared units of the target, which is hard to picture, so the RMSE, its square root, is in the original units. R squared compares the model with the simplest baseline of always predicting the mean. A value of 1 is perfect, 0 is no better than the mean, and negative is worse. A value around 0.45 on this dataset means the model explains a bit under half of the variation, which is typical for this noisy medical data.

### 1.3 Predicted against actual

A plot tells you more than a single score. If predictions were perfect, all points would lie on the diagonal.

```python
import matplotlib.pyplot as plt

plt.scatter(y_test, y_pred)
lims = [y_test.min(), y_test.max()]
plt.plot(lims, lims, color="black", linestyle="--")
plt.xlabel("actual")
plt.ylabel("predicted")
plt.title("Linear regression on the diabetes data")
plt.show()
```

Look for patterns. If the points curve away from the diagonal, a straight line is not the right model. If the spread grows with the value, errors are not uniform. These observations point to what to try next.

## 2. Logistic Regression

Despite the name, logistic regression is a classification method. It handles the binary case, where the label is 0 or 1. We still compute a linear combination `z = w.x + b`, but then pass it through the sigmoid function to obtain a probability between 0 and 1.

```
P(y=1 | x) = 1 / (1 + exp(-(w.x + b)))
```

```python
def sigmoid(z):
    return 1 / (1 + np.exp(-z))

for z in [-6, -2, 0, 2, 6]:
    print(f"z = {z:2d}  sigmoid = {sigmoid(z):.3f}")
```

Large negative inputs go towards 0, large positive ones towards 1, and zero maps to exactly 0.5. The decision boundary is where `P(y=1 | x) = 0.5`, which is where `z = 0`. In two dimensions it is a straight line.

Logistic regression is trained by minimizing a different cost, the log loss (cross entropy), which punishes confident wrong answers heavily. There is no closed form solution, so scikit-learn uses an iterative optimizer.

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

cancer = load_breast_cancer()
Xc_train, Xc_test, yc_train, yc_test = train_test_split(
    cancer.data, cancer.target, test_size=0.2, random_state=42, stratify=cancer.target
)

clf = LogisticRegression(max_iter=5000)
clf.fit(Xc_train, yc_train)
pred = clf.predict(Xc_test)
print("accuracy:", round(accuracy_score(yc_test, pred), 3))
print("first five probabilities:", clf.predict_proba(Xc_test[:5])[:, 1].round(3))
```

`predict` gives the class, which is simply whether the probability exceeds 0.5. `predict_proba` gives the probabilities themselves, which are often more useful, since they tell you how sure the model is. For example, a medical system may want to flag any case above 0.2 for review.

The `max_iter=5000` argument is needed here because the features have very different scales, and the optimizer needs more iterations to converge. This brings us to scaling.

## 3. Feature Scaling

Models that rely on gradients or distances are sensitive to the units of the features. If one feature ranges from 0 to 1 and another from 0 to 100,000, the large one will dominate distances and make gradient descent slow and unstable.

Standardization rescales each feature to mean 0 and standard deviation 1.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
Xc_train_scaled = scaler.fit_transform(Xc_train)
Xc_test_scaled = scaler.transform(Xc_test)   # use the SAME scaler, fit on training data

print("train means:", Xc_train_scaled.mean(axis=0)[:3].round(2))
print("train stds: ", Xc_train_scaled.std(axis=0)[:3].round(2))
```

An important rule: fit the scaler only on the training data, then apply it to both training and test data. If you fit it on the test data, or on everything, information about the test set leaks into preprocessing. This is the leakage problem from Week 9, and it makes results look better than they will be in reality.

Compare the logistic regression with and without scaling.

```python
clf_raw = LogisticRegression(max_iter=5000).fit(Xc_train, yc_train)
clf_scaled = LogisticRegression(max_iter=5000).fit(Xc_train_scaled, yc_train)

print("raw features, iterations used:   ", clf_raw.n_iter_[0])
print("scaled features, iterations used:", clf_scaled.n_iter_[0])
print("raw accuracy:   ", round(clf_raw.score(Xc_test, yc_test), 3))
print("scaled accuracy:", round(clf_scaled.score(Xc_test_scaled, yc_test), 3))
```

The scaled version converges in far fewer iterations and usually gives equal or better accuracy.

### 3.1 Pipelines

Remembering to apply the right transformation to every set is error prone. A pipeline bundles the steps, so that `fit` only ever sees training data and `predict` applies the same transformations automatically.

```python
from sklearn.pipeline import make_pipeline

pipe = make_pipeline(StandardScaler(), LogisticRegression(max_iter=1000))
pipe.fit(Xc_train, yc_train)
print("pipeline accuracy:", round(pipe.score(Xc_test, yc_test), 3))
```

From now on, prefer pipelines whenever preprocessing is involved. They also work correctly inside cross-validation, where leakage is otherwise easy to introduce.

## 4. Interpreting Coefficients

For linear regression, `coef_[i]` is the change in the predicted target for a one-unit increase in feature `i`, holding the other features fixed.

Two cautions apply.

1. The size of a coefficient depends on the units of its feature. A coefficient for height in metres will be a hundred times larger than the one for height in centimetres. Comparing raw coefficients across features of different scales is meaningless. After standardizing, a coefficient tells you the change in the target per standard deviation of the feature, which is a fairer comparison.
2. "Holding the other features fixed" matters when features are correlated. If two features carry almost the same information, the model may split the credit between them in an unstable way, and their signs and sizes can change sharply with small changes in the data.

```python
pipe_lr = make_pipeline(StandardScaler(), LinearRegression())
pipe_lr.fit(X_train, y_train)
coefs = pipe_lr.named_steps["linearregression"].coef_

order = np.argsort(-np.abs(coefs))
for i in order:
    print(f"{data.feature_names[i]:>4s} {coefs[i]:8.2f}")
```

The features with the largest absolute coefficients are `s1`, `s5` and `bmi`. Notice that `s1` is strongly negative while its close relative `s2` is positive, which is a hint of the correlation problem discussed next. All of this is a statement about association in this dataset, not about cause.

Check the correlation problem directly. Features `s1` and `s2` in this dataset are strongly correlated.

```python
corr = np.corrcoef(data.data[:, 4], data.data[:, 5])[0, 1]
print("correlation between s1 and s2:", round(corr, 2))
```

When features are this correlated, the individual coefficients are not reliable, even though the predictions can still be good.

## 5. Worked Example: Regression Compared with a Baseline

An evaluation without a baseline is hard to interpret. A good habit is to compare with the simplest possible predictor.

```python
from sklearn.dummy import DummyRegressor

baseline = DummyRegressor(strategy="mean").fit(X_train, y_train)
base_rmse = mean_squared_error(y_test, baseline.predict(X_test)) ** 0.5
model_rmse = mean_squared_error(y_test, model.predict(X_test)) ** 0.5

print(f"baseline RMSE (always predict the mean): {base_rmse:.1f}")
print(f"linear regression RMSE:                  {model_rmse:.1f}")
```

If your model barely beats the baseline, you have learned that the features carry little signal, or that your model is the wrong type.

## 6. In-Class Exercise

Fit a `LinearRegression` model on a provided dataset, plot predicted against actual values, and fit a `LogisticRegression` model on a binary dataset, reporting test accuracy.

You can complete it with the datasets used above. A suggested solution outline:

```python
# regression part: already done above (model, y_test, y_pred)
print("regression R^2:", round(r2_score(y_test, y_pred), 3))

# classification part with a pipeline
pipe = make_pipeline(StandardScaler(), LogisticRegression(max_iter=1000))
pipe.fit(Xc_train, yc_train)
print("classification accuracy:", round(pipe.score(Xc_test, yc_test), 3))

# a majority class baseline for context
from sklearn.dummy import DummyClassifier
dummy = DummyClassifier(strategy="most_frequent").fit(Xc_train, yc_train)
print("majority class baseline:", round(dummy.score(Xc_test, yc_test), 3))
```

Questions:

1. How much better than the majority baseline is the logistic regression? Is accuracy a satisfying measure here? We study better measures next week.
2. Change `alpha` in the manual gradient descent, and plot the cost history for three rates.
3. Add the squared value of `x` as an extra feature to the manual example. Does a linear model become able to fit a curve? (Yes, since it is linear in the weights, not in the input.)

## 7. Common Mistakes

1. Fitting the scaler on the full dataset before splitting.
2. Comparing raw coefficients across features with different units.
3. Reading a coefficient as a causal effect.
4. Using too large a learning rate and not noticing that the cost is growing.
5. Reporting a score without a baseline for comparison.
6. Forgetting that logistic regression is a classifier despite its name.

## 8. Summary

Linear regression predicts a number from a weighted sum of features, and it is trained by minimizing the mean squared error. Gradient descent is the general method behind most model training, and scikit-learn solves this particular case directly. Logistic regression passes a linear score through the sigmoid to produce a probability, and is used for binary classification. Scaling features matters, but only when it is done in the right order, which pipelines enforce for us.

## 9. Practice Problems

1. Implement gradient descent for two features, using matrix notation: `grad = (2/n) * X.T @ (X @ w - y)`.
2. On the diabetes data, compare the test RMSE of `LinearRegression`, `Ridge` and `Lasso`, and describe how the coefficients differ.
3. Plot the decision boundary of a logistic regression on two features of the breast cancer data.
4. Show experimentally that fitting the scaler on the full dataset changes the test accuracy, even slightly, compared with the correct approach, for a small dataset.

## 10. Suggested Reading

1. The scikit-learn user guide pages on linear models and on preprocessing.
2. Géron, Hands-On Machine Learning, the chapter on training models.

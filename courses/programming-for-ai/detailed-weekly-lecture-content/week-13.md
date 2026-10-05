# Week 13: Model Evaluation, Overfitting and Hyperparameter Tuning

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain the bias-variance tradeoff and recognise underfitting and overfitting from scores.
2. Use k-fold cross-validation to estimate performance more reliably than one split.
3. Tune hyperparameters with `GridSearchCV` and `RandomizedSearchCV` without touching the test set.
4. Describe L1 and L2 regularization and observe their effect on a model.
5. Plot and read a learning curve to decide what to do next.

## 1. The Bias-Variance Tradeoff

Every model makes errors, and these errors come from two quite different sources.

1. Bias is error from a model that is too simple to represent the true pattern. A straight line fitted to data that follows a curve will be wrong in a systematic way, however much data you give it. This is underfitting. The model performs badly on both the training and the test data.
2. Variance is error from a model that is too sensitive to the particular sample it was trained on. A very flexible model fits the quirks and noise of its training data, so a different training sample would give a very different model. This is overfitting. The model does very well on the training data and noticeably worse on new data.

The aim is not the lowest training error. The aim is the lowest error on unseen data, and that usually lies between the two extremes. Increasing the flexibility of a model lowers bias but raises variance, and the best model balances them.

### 1.1 Seeing it with polynomial regression

We generate data from a smooth curve with noise, and fit polynomials of increasing degree. The degree is the dial for flexibility.

```python
import numpy as np
from sklearn.preprocessing import PolynomialFeatures, StandardScaler
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import make_pipeline
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

rng = np.random.default_rng(0)
x = np.sort(rng.uniform(0, 1, 40))
y = np.sin(2 * np.pi * x) + rng.normal(0, 0.25, size=x.size)
X = x.reshape(-1, 1)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=1)

print("degree  train MSE  test MSE")
for degree in [1, 3, 5, 9, 12]:
    model = make_pipeline(PolynomialFeatures(degree), StandardScaler(), LinearRegression())
    model.fit(X_train, y_train)
    tr = mean_squared_error(y_train, model.predict(X_train))
    te = mean_squared_error(y_test, model.predict(X_test))
    print(f"{degree:6d}  {tr:9.3f}  {te:8.3f}")
```

Read the table column by column. The training error falls steadily as the degree rises, because a more flexible curve can pass closer to the training points. The test error drops sharply from degree 1 to degree 3, stays roughly flat for a while, and then jumps to 3.9 at degree 12. Degree 1 underfits, since a line cannot follow a sine wave. Degree 12 overfits: with 24 training points it has more freedom than the data can pin down, so it chases the noise. Degree 3 is the sensible choice here, and it is not the one with the lowest training error.

## 2. k-Fold Cross-Validation

A single train and test split is a noisy measure. With a small dataset, a lucky or unlucky split can change the score considerably. We also cannot keep peeking at the test set to choose between models, since this makes the test score optimistic.

k-fold cross-validation uses the training data more thoroughly. It splits the training data into `k` folds. It trains on `k - 1` folds and scores on the remaining one, and repeats this `k` times so that every fold serves once as the validation set. The `k` scores are then averaged. The test set stays untouched.

```python
from sklearn.datasets import load_breast_cancer
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import cross_val_score

data = load_breast_cancer()
Xc_train, Xc_test, yc_train, yc_test = train_test_split(
    data.data, data.target, test_size=0.25, random_state=42, stratify=data.target
)

model = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=5))
scores = cross_val_score(model, Xc_train, yc_train, cv=5, scoring="accuracy")

print("fold scores:", scores.round(3))
print("mean:", scores.mean().round(3), " std:", scores.std().round(3))
```

The spread across folds, the standard deviation, tells you how much to trust the mean. If two models differ by less than the spread, you cannot say that one is really better.

Because we passed a pipeline, the scaler is refitted inside each fold using only that fold's training part. This avoids the leakage problem from Weeks 9 and 10. If you scaled the data first and then cross-validated, each validation fold would have influenced the scaler.

For classification, `cv=5` automatically uses stratified folds, so each fold keeps the class proportions. For time series data you must not shuffle, and should use `TimeSeriesSplit` instead, so that the model is never trained on the future.

## 3. Hyperparameter Tuning

A parameter is learned from data, such as a regression weight. A hyperparameter is a setting we choose before training, such as `k` in k-NN, the depth of a tree, or the strength of regularization. Cross-validation is the standard way to choose them.

`GridSearchCV` tries every combination in a grid, scores each with cross-validation, and keeps the best. It then refits the best combination on the whole training set.

```python
from sklearn.model_selection import GridSearchCV

pipe = make_pipeline(StandardScaler(), KNeighborsClassifier())
param_grid = {
    "kneighborsclassifier__n_neighbors": [1, 3, 5, 7, 9, 15],
    "kneighborsclassifier__weights": ["uniform", "distance"],
}

grid = GridSearchCV(pipe, param_grid, cv=5, scoring="accuracy")
grid.fit(Xc_train, yc_train)

print("best parameters:", grid.best_params_)
print("best cv score:  ", round(grid.best_score_, 3))
print("test score:     ", round(grid.score(Xc_test, yc_test), 3))
```

Pipeline parameters are named as `stepname__parametername`, with a double underscore. The test set is used exactly once, in the last line, to report the final score of the chosen configuration.

We can inspect all the tried combinations.

```python
import pandas as pd

results = pd.DataFrame(grid.cv_results_)
cols = ["param_kneighborsclassifier__n_neighbors", "param_kneighborsclassifier__weights",
        "mean_test_score", "std_test_score"]
print(results[cols].sort_values("mean_test_score", ascending=False).head(5).round(3).to_string(index=False))
```

Often several settings are tied within the noise. In that case choose the simplest one.

### 3.1 Randomized search

A grid grows multiplicatively. Five hyperparameters with ten values each means 100,000 combinations, each needing `k` model fits. `RandomizedSearchCV` samples a fixed number of random combinations and often finds a nearly as good one at a fraction of the cost.

```python
from sklearn.model_selection import RandomizedSearchCV
from sklearn.ensemble import RandomForestClassifier
from scipy.stats import randint

param_dist = {
    "n_estimators": randint(20, 200),
    "max_depth": randint(2, 12),
    "min_samples_leaf": randint(1, 10),
}
search = RandomizedSearchCV(
    RandomForestClassifier(random_state=0), param_dist,
    n_iter=15, cv=5, random_state=0
)
search.fit(Xc_train, yc_train)
print("best parameters:", search.best_params_)
print("best cv score:  ", round(search.best_score_, 3))
print("test score:     ", round(search.score(Xc_test, yc_test), 3))
```

Even though we did not discuss random forests in detail, the code shows the pattern: any estimator can be tuned this way.

## 4. Regularization

Regularization fights overfitting by penalizing complexity directly. For linear models, large weights usually mean the model is bending itself to fit noise. So we add a penalty on the size of the weights to the loss.

```
Loss_regularized = Loss + alpha * sum(|w|)        # L1, Lasso
Loss_regularized = Loss + alpha * sum(w^2)        # L2, Ridge
```

The hyperparameter `alpha` sets the strength. At zero there is no penalty, and as it grows the weights are pushed towards zero.

1. L2 (Ridge) shrinks all weights smoothly, and rarely makes any exactly zero.
2. L1 (Lasso) tends to set some weights exactly to zero, which performs a kind of feature selection.

We apply them to the overfitting polynomial from before.

```python
from sklearn.linear_model import Ridge, Lasso

degree = 12
print("model                      train MSE  test MSE   largest |weight|")
for name, reg in [("no regularization", LinearRegression()),
                  ("Ridge alpha=0.1", Ridge(alpha=0.1)),
                  ("Ridge alpha=1", Ridge(alpha=1.0)),
                  ("Lasso alpha=0.01", Lasso(alpha=0.01, max_iter=100000))]:
    m = make_pipeline(PolynomialFeatures(degree), StandardScaler(), reg)
    m.fit(X_train, y_train)
    tr = mean_squared_error(y_train, m.predict(X_train))
    te = mean_squared_error(y_test, m.predict(X_test))
    w = np.abs(m[-1].coef_).max()
    print(f"{name:25s}  {tr:8.3f}  {te:8.3f}   {w:10.2f}")
```

The unregularized degree 12 model has very large weights and a poor test error. The regularized models use much smaller weights and generalize better, even though their training error is slightly worse. Giving up a little training accuracy to gain test accuracy is the entire idea.

We should choose `alpha` by cross-validation, like any other hyperparameter. scikit-learn provides a shortcut.

```python
from sklearn.linear_model import RidgeCV

best = make_pipeline(PolynomialFeatures(12), StandardScaler(),
                     RidgeCV(alphas=np.logspace(-4, 2, 30)))
best.fit(X_train, y_train)
print("chosen alpha:", round(best[-1].alpha_, 4))
print("test MSE:", round(mean_squared_error(y_test, best.predict(X_test)), 3))
```

In scikit-learn's `LogisticRegression` the parameter `C` is the inverse of the regularization strength. A small `C` means stronger regularization. Reading the documentation for the convention of each model before tuning is a good habit.

## 5. Learning Curves

A learning curve shows training and validation scores as the amount of training data grows. It diagnoses what is wrong and what to do next.

1. Both curves are poor and close together: underfitting, or high bias. More data will not help much. Use a more expressive model or better features.
2. A big gap between a high training score and a lower validation score: overfitting, or high variance. More data, regularization or a simpler model will help.
3. Both curves are high and converge: the model is well fitted.

```python
from sklearn.model_selection import learning_curve
import matplotlib.pyplot as plt

def plot_curve(model, X, y, title):
    sizes, train_scores, val_scores = learning_curve(
        model, X, y, cv=5, train_sizes=np.linspace(0.1, 1.0, 8), scoring="accuracy", random_state=0, shuffle=True
    )
    plt.plot(sizes, train_scores.mean(axis=1), marker="o", color="black", label="training")
    plt.plot(sizes, val_scores.mean(axis=1), marker="s", linestyle="--", color="gray", label="validation")
    plt.xlabel("training set size")
    plt.ylabel("accuracy")
    plt.title(title)
    plt.legend()
    plt.show()
    return sizes, train_scores.mean(axis=1), val_scores.mean(axis=1)

from sklearn.tree import DecisionTreeClassifier

sizes, tr, va = plot_curve(DecisionTreeClassifier(random_state=0), Xc_train, yc_train, "Unrestricted decision tree")
print("final training score:  ", tr[-1].round(3))
print("final validation score:", va[-1].round(3))
```

An unrestricted tree gets a training score of 1.0, while the validation score stays lower. The gap indicates overfitting. Now compare with a restricted tree and with logistic regression.

```python
sizes, tr2, va2 = plot_curve(DecisionTreeClassifier(max_depth=3, random_state=0), Xc_train, yc_train, "Tree with depth 3")
print("depth 3: train", tr2[-1].round(3), " validation", va2[-1].round(3))
```

Now logistic regression. We need to import it first.

```python
from sklearn.linear_model import LogisticRegression

lr = make_pipeline(StandardScaler(), LogisticRegression(max_iter=2000))
sizes, tr3, va3 = plot_curve(lr, Xc_train, yc_train, "Logistic regression")
print("logistic: train", tr3[-1].round(3), " validation", va3[-1].round(3))
```

Compare the three. The unrestricted tree has training accuracy 1.0 and validation accuracy about 0.92, a gap of eight points. The tree of depth 3 has 0.98 against 0.93, a smaller gap and a better validation score. Logistic regression has about 0.99 against 0.97, so it is close to well fitted on this dataset, and the room left for improvement is small.

## 6. Worked Example: A Complete Honest Evaluation

This pulls everything together in the proper order.

```python
from sklearn.svm import SVC

# 1. hold out the test set, and never look at it until the end
X_dev, X_final, y_dev, y_final = train_test_split(
    data.data, data.target, test_size=0.2, random_state=7, stratify=data.target
)

# 2. compare candidate models with cross-validation on the development data
candidates = {
    "logistic": make_pipeline(StandardScaler(), LogisticRegression(max_iter=2000)),
    "knn": make_pipeline(StandardScaler(), KNeighborsClassifier()),
    "svm": make_pipeline(StandardScaler(), SVC()),
    "tree": DecisionTreeClassifier(max_depth=4, random_state=0),
}
for name, m in candidates.items():
    s = cross_val_score(m, X_dev, y_dev, cv=5)
    print(f"{name:9s} cv accuracy {s.mean():.3f} +/- {s.std():.3f}")

# 3. tune the winner
svm_grid = GridSearchCV(
    make_pipeline(StandardScaler(), SVC()),
    {"svc__C": [0.1, 1, 10, 100], "svc__gamma": ["scale", 0.01, 0.001]},
    cv=5
).fit(X_dev, y_dev)
print("best svm:", svm_grid.best_params_, round(svm_grid.best_score_, 3))

# 4. one final look at the untouched test set
print("final test accuracy:", round(svm_grid.score(X_final, y_final), 3))
```

The steps are the template for the capstone project. The test set was used exactly once, and the score reported at the end is an honest estimate of how the model should perform on new data.

## 7. In-Class Exercise

Run `cross_val_score` and `GridSearchCV` on a model from Week 11, plot a learning curve, and diagnose whether the current model underfits, overfits, or is well fitted.

Use the code in sections 2, 3 and 5. Write your diagnosis in one paragraph covering: the cross-validated score and its spread, the gap between training and validation scores on the learning curve, and one concrete action you would take next. For example: "The unrestricted tree has a training accuracy of 1.0 against a validation accuracy near 0.92, so it overfits. Limiting the depth, or using more data, should help. Cross-validation of a depth limited tree confirms a smaller gap."

Questions:

1. Why is it wrong to report `best_score_` from a grid search as the final performance of the model?
2. How many model fits does the grid in section 3 perform, including the final refit?
3. What would you expect to happen to the learning curve of k-NN with `k = 1`?

## 8. Common Mistakes

1. Choosing the model with the test set, and then reporting the same test score.
2. Preprocessing before cross-validation, which leaks information between folds.
3. Reading a difference between two cross-validated scores as meaningful when it is smaller than the spread.
4. Searching a huge grid on a small dataset, which can itself overfit the validation data.
5. Forgetting that `C` in logistic regression and SVM is an inverse strength.
6. Shuffling time series data before splitting.

## 9. Summary

Models can fail by being too simple or too flexible, and the tools of this week let us tell which has happened. Cross-validation gives a steadier estimate of performance. Grid and randomized search tune hyperparameters using that estimate while keeping the test set pristine. Regularization trades a little training accuracy for better generalization, and learning curves tell us whether more data, a different model or more regularization is the right next move.

## 10. Practice Problems

1. For the polynomial example, use cross-validation to select the best degree, and then evaluate on the test set.
2. Plot the effect of `alpha` on the coefficients of a Lasso model for the diabetes dataset, and find which features are dropped first.
3. Use `RandomizedSearchCV` with `loguniform` for `C` and `gamma` of an SVM, and compare it with a grid search in terms of time and score.
4. Produce a learning curve for a model of your choice on the wine dataset, and write a three sentence diagnosis.

## 11. Suggested Reading

1. The scikit-learn user guide chapters on cross-validation, tuning and learning curves.
2. Hastie, Tibshirani and Friedman, The Elements of Statistical Learning, the chapter on model assessment and selection.

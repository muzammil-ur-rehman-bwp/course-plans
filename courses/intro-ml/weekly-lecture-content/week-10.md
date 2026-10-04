# Week 10 — Lecture Content: Model Evaluation & Selection in Depth

## 1. Why One Train/Test Split Isn't Enough
A single train/test split gives one noisy estimate of generalization performance — it depends
on exactly which examples happened to land in the test set. **k-fold cross-validation** splits
the training data into `k` folds, trains on `k-1` folds and validates on the remaining fold, `k`
times (each fold used once as validation), then averages the `k` scores for a more stable
estimate.
```python
from sklearn.model_selection import cross_val_score, StratifiedKFold
from sklearn.ensemble import RandomForestClassifier

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(RandomForestClassifier(random_state=42), X_train, y_train, cv=cv)
print("Mean accuracy:", scores.mean(), "Std:", scores.std())
```
`StratifiedKFold` preserves each fold's class proportions — important for imbalanced
classification, where plain `KFold` could produce folds with very different class ratios.

## 2. Nested Cross-Validation (Brief)
If hyperparameters are tuned using cross-validation on the **same** data later used to report
the final cross-validated score, that score is optimistically biased — the tuning process has
already "seen" that data indirectly. **Nested cross-validation** uses an outer loop for honest
performance estimation and an inner loop (within each outer training fold) for hyperparameter
selection:
```python
from sklearn.model_selection import GridSearchCV, cross_val_score

inner_cv = StratifiedKFold(n_splits=3, shuffle=True, random_state=1)
outer_cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=2)

grid = GridSearchCV(RandomForestClassifier(random_state=42),
                     param_grid={"max_depth": [3, 5, 10, None]}, cv=inner_cv)
nested_scores = cross_val_score(grid, X_train, y_train, cv=outer_cv)
print("Nested CV honest estimate:", nested_scores.mean())
```
Nested CV is more computationally expensive and is typically reserved for final, rigorous
reporting (e.g., in the capstone) rather than everyday model iteration.

## 3. GridSearchCV and RandomizedSearchCV
`GridSearchCV` exhaustively tries every combination in a specified hyperparameter grid, scoring
each by cross-validation, and refits the best combination on the full training data:
```python
param_grid = {"n_estimators": [100, 200, 400], "max_depth": [5, 10, None]}
grid_search = GridSearchCV(RandomForestClassifier(random_state=42), param_grid, cv=5, scoring="f1")
grid_search.fit(X_train, y_train)
print(grid_search.best_params_, grid_search.best_score_)
```
`RandomizedSearchCV` samples a fixed number of random combinations from specified distributions
instead of trying all of them — far cheaper when the grid is large or some hyperparameters are
continuous:
```python
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import randint

param_dist = {"n_estimators": randint(50, 500), "max_depth": randint(2, 30)}
rand_search = RandomizedSearchCV(RandomForestClassifier(random_state=42), param_dist,
                                  n_iter=20, cv=5, random_state=42)
rand_search.fit(X_train, y_train)
```

## 4. Learning Curves
A **learning curve** plots training and validation score against training set size (or, for a
fixed model, against a complexity hyperparameter). It diagnoses:
- **High bias (underfitting):** both curves converge to a low score; more data won't help much —
  a more flexible model or better features is needed.
- **High variance (overfitting):** a large gap between a high training score and a lower
  validation score; more data, regularization, or a simpler model would help.
```python
from sklearn.model_selection import learning_curve
import numpy as np, matplotlib.pyplot as plt

train_sizes, train_scores, val_scores = learning_curve(
    RandomForestClassifier(random_state=42), X_train, y_train,
    train_sizes=np.linspace(0.1, 1.0, 10), cv=5
)
plt.plot(train_sizes, train_scores.mean(axis=1), label="Training score")
plt.plot(train_sizes, val_scores.mean(axis=1), label="Validation score")
plt.xlabel("Training set size"); plt.ylabel("Score"); plt.legend(); plt.show()
```

## 5. The Bias-Variance Tradeoff, Quantitatively
For squared-error loss, the expected test error at a point decomposes as:
```
E[(y - f_hat(x))^2] = Bias[f_hat(x)]^2 + Var[f_hat(x)] + sigma^2
```
- **Bias:** error from the model's assumptions being wrong (too simple a model).
- **Variance:** error from the model's sensitivity to the particular training sample (too
  flexible a model).
- **sigma^2 (irreducible error):** noise inherent to the problem, which no model can remove.
Total expected error is minimized at some intermediate model complexity — simpler models have
higher bias/lower variance, more complex models have lower bias/higher variance, and the best
achievable test error balances the two against the fixed noise floor.

## 6. In-Class Exercise
Given a learning curve showing training score ~0.98 and validation score ~0.75 that is not
closing as training size increases, diagnose the problem and propose two concrete remedies.

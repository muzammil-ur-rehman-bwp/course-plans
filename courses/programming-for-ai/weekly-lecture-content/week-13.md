# Week 13 — Lecture Content: Model Evaluation, Overfitting, Hyperparameter Tuning

## 1. Bias-Variance Tradeoff
- **High bias (underfitting)**: model is too simple, performs poorly on both train and test data.
- **High variance (overfitting)**: model fits training data very well but generalizes poorly to
  unseen data — it has learned noise, not signal.
- Goal: find the sweet spot that minimizes test error, not training error.

## 2. k-Fold Cross-Validation
Splits the training data into `k` folds; trains on `k-1` folds and validates on the remaining
fold, rotating which fold is held out, then averages the validation scores. More robust than a
single train/validation split, especially with limited data.
```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(model, X_train, y_train, cv=5, scoring="accuracy")
print(scores.mean(), scores.std())
```

## 3. Hyperparameter Tuning
```python
from sklearn.model_selection import GridSearchCV

param_grid = {"n_neighbors": [3, 5, 7, 9]}
grid = GridSearchCV(KNeighborsClassifier(), param_grid, cv=5)
grid.fit(X_train, y_train)
print(grid.best_params_, grid.best_score_)
```
`GridSearchCV` exhaustively tries every combination; `RandomizedSearchCV` samples a subset of
combinations, useful when the hyperparameter space is large.

## 4. Regularization (Introductory)
L1 (Lasso) and L2 (Ridge) regularization add a penalty on model weight magnitude to the loss
function, discouraging overly complex models and reducing overfitting:
```
Loss_regularized = Loss + alpha * sum(|w|)        # L1
Loss_regularized = Loss + alpha * sum(w^2)        # L2
```

## 5. Learning Curves
Plotting training vs. validation error against training-set size reveals the fit regime:
- Both errors high, close together → underfitting (more data won't help much; need a more
  expressive model or better features).
- Large gap between low training error and high validation error → overfitting (more data,
  regularization, or a simpler model would help).

## 6. In-Class Exercise
Run `cross_val_score` and `GridSearchCV` on a model from Week 11; plot a learning curve and
diagnose whether the current model underfits, overfits, or is well-fit.

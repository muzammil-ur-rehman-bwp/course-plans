# Week 8 — Lecture Content: Ensemble Methods II — Boosting; Midterm Review

## 1. Boosting Intuition
Where bagging trains independent models in parallel on bootstrap samples, **boosting** trains a
sequence of typically weak models, each one focusing on the examples the previous models got
wrong. Boosting primarily reduces **bias** (it can turn an ensemble of weak learners into a
strong one), while bagging primarily reduces **variance**.

## 2. AdaBoost (Conceptual)
AdaBoost ("Adaptive Boosting") maintains a weight for each training example, starting uniform.
At each round:
1. Fit a weak learner (commonly a shallow "stump" — a depth-1 decision tree) on the weighted
   data.
2. Increase the weight of examples it misclassified, decrease the weight of examples it got
   right.
3. Add the new learner to the ensemble with a weight proportional to its accuracy.
The final prediction is a weighted vote of all weak learners.
```python
from sklearn.ensemble import AdaBoostClassifier
from sklearn.tree import DecisionTreeClassifier

ada = AdaBoostClassifier(
    estimator=DecisionTreeClassifier(max_depth=1),
    n_estimators=200,
    random_state=42,
)
ada.fit(X_train, y_train)
```

## 3. Gradient Boosting
Gradient Boosting generalizes the boosting idea: each new weak learner is fit to approximate the
**negative gradient of the loss function** with respect to the current ensemble's predictions —
for squared-error loss, this is simply the current **residual** (`y - y_hat`). Each new tree's
predictions, scaled by a learning rate, are added to the ensemble:
```
F_m(x) = F_{m-1}(x) + lr * h_m(x)
```
where `h_m` is the new weak learner fit to the residuals/gradient at step `m`.
```python
from sklearn.ensemble import GradientBoostingClassifier

gb = GradientBoostingClassifier(
    n_estimators=200, learning_rate=0.1, max_depth=3, random_state=42
)
gb.fit(X_train, y_train)
print("Train:", gb.score(X_train, y_train), "Test:", gb.score(X_test, y_test))
```
A smaller learning rate with more estimators generally generalizes better than a large learning
rate with few estimators, at the cost of more training time — a direct bias-variance/compute
tradeoff.

## 4. XGBoost and LightGBM (Brief)
**XGBoost** and **LightGBM** are real, widely-used open-source libraries that implement highly
optimized, regularized variants of gradient boosting (faster training, built-in handling of
missing values, and additional regularization terms on tree complexity). They are not required
for this course's labs or capstone but are worth knowing by name as the production-grade tools
most practitioners reach for when scikit-learn's `GradientBoostingClassifier` is too slow for
large datasets; their APIs are designed to be largely compatible with scikit-learn's conventions
(`fit`/`predict`).

## 5. Boosting vs. Bagging — Bias/Variance Behavior
| | Bagging / Random Forest | Boosting (AdaBoost, Gradient Boosting) |
|---|---|---|
| Base learners trained | In parallel, independently | Sequentially, each depending on the last |
| Primarily reduces | Variance | Bias |
| Overfitting risk | Low, even with many estimators | Can overfit with too many estimators / too high a learning rate |
| Typical base learner | Fully-grown or lightly-constrained tree | Shallow tree ("stump" or depth 3–5) |

```python
# Compare train/test accuracy across the three ensemble families on the same data
for name, model in [("Bagging", bagged), ("Random Forest", rf), ("AdaBoost", ada), ("Gradient Boosting", gb)]:
    print(name, "train:", model.score(X_train, y_train), "test:", model.score(X_test, y_test))
```

## 6. Midterm Review
The midterm (Week 9) covers Weeks 1–8: the ML workflow and bias-variance tradeoff (Week 1); linear
and regularized regression (Weeks 2–3); logistic regression and classification metrics (Week 4);
k-NN and Naive Bayes (Week 5); decision trees (Week 6); and bagging/Random Forests/boosting
(Weeks 7–8). Review focuses on deriving cost functions and splitting criteria by hand, and on
correctly selecting and justifying a model/metric for a described scenario.

## 7. In-Class Exercise
Fit AdaBoost and Gradient Boosting on the Week 6–7 dataset; compare their test accuracy and
train/test gap to the Week 7 bagging and Random Forest results.

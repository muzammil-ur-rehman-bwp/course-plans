# Week 11 — Lecture Content: Classification & Model Evaluation Metrics

## 1. k-Nearest Neighbors (k-NN)
Classifies a new point by majority vote among its `k` closest training points (by distance,
typically Euclidean). No explicit training phase ("lazy learner") — all computation happens at
prediction time.
```python
from sklearn.neighbors import KNeighborsClassifier

knn = KNeighborsClassifier(n_neighbors=5)
knn.fit(X_train, y_train)
y_pred = knn.predict(X_test)
```
Choice of `k` matters: small `k` → sensitive to noise (overfitting risk); large `k` → oversmoothed
decision boundary (underfitting risk).

## 2. Decision Trees & SVM (brief)
- **Decision Tree**: recursively splits the feature space on the feature/threshold that best
  separates classes (e.g., by Gini impurity). Easy to interpret, prone to overfitting if too deep.
- **SVM (Support Vector Machine)**: finds the hyperplane that maximizes the margin between
  classes; kernels (e.g., RBF) allow non-linear decision boundaries.
```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.svm import SVC

tree = DecisionTreeClassifier(max_depth=4).fit(X_train, y_train)
svm = SVC(kernel="rbf").fit(X_train, y_train)
```

## 3. Evaluation Metrics
| Metric | Formula | Use when |
|---|---|---|
| Accuracy | (TP+TN)/(TP+TN+FP+FN) | Classes are roughly balanced |
| Precision | TP/(TP+FP) | Cost of false positives is high |
| Recall | TP/(TP+FN) | Cost of false negatives is high |
| F1 | 2·Precision·Recall/(Precision+Recall) | Need a single balance of precision/recall |

```python
from sklearn.metrics import confusion_matrix, classification_report

print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred))
```

## 4. ROC/AUC
The ROC curve plots True Positive Rate vs. False Positive Rate across classification thresholds;
AUC (Area Under the Curve) summarizes performance into a single number (1.0 = perfect,
0.5 = random guessing).
```python
from sklearn.metrics import roc_curve, roc_auc_score

probs = clf.predict_proba(X_test)[:, 1]
fpr, tpr, thresholds = roc_curve(y_test, probs)
auc = roc_auc_score(y_test, probs)
```

## 5. In-Class Exercise
Train k-NN, Decision Tree, and SVM on the same dataset; produce a classification report for each
and discuss which metric matters most for the given problem (and why accuracy alone can mislead
on imbalanced data).

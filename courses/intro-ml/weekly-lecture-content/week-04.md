# Week 4 — Lecture Content: Logistic Regression & Classification Metrics in Depth

## 1. The Sigmoid Function and Decision Boundary
Logistic regression models the probability of the positive class as a sigmoid applied to a
linear score:
```
z = w^T x + b
P(y=1 | x) = sigma(z) = 1 / (1 + exp(-z))
```
`sigma(z)` maps any real number to `(0, 1)`. The default decision rule predicts class 1 when
`P(y=1|x) >= 0.5`, which is equivalent to `z >= 0` — a **linear decision boundary** in `x`-space.
The log-odds (logit) is linear in the features: `log(P / (1-P)) = w^T x + b`, which is why
coefficients are interpreted as the change in log-odds per unit change in a feature.

```python
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression(max_iter=1000)
clf.fit(X_train, y_train)
y_pred = clf.predict(X_test)
y_prob = clf.predict_proba(X_test)[:, 1]   # probability of the positive class
```

## 2. Precision, Recall, F1, and the Confusion Matrix
For binary classification with a "positive" class, the confusion matrix has four cells: true
positives (TP), false positives (FP), true negatives (TN), false negatives (FN).
```
Precision = TP / (TP + FP)      "of predicted positives, how many were correct"
Recall    = TP / (TP + FN)      "of actual positives, how many were found"
F1        = 2 * Precision * Recall / (Precision + Recall)   (harmonic mean)
```
Accuracy `(TP+TN)/(TP+TN+FP+FN)` can be misleading on imbalanced data: a classifier that always
predicts the majority class can have high accuracy and zero recall for the minority class.
Precision matters more when false positives are costly (e.g., flagging legitimate email as spam);
recall matters more when false negatives are costly (e.g., missing a disease diagnosis).

```python
from sklearn.metrics import confusion_matrix, classification_report

print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred))
```

## 3. ROC Curves and AUC
The ROC curve plots the **true positive rate** (recall) against the **false positive rate**
(`FP / (FP + TN)`) as the decision threshold sweeps from 0 to 1, independent of any single
threshold choice. The **Area Under the Curve (AUC)** summarizes ranking quality: AUC = 0.5 is no
better than random ranking; AUC = 1.0 is perfect ranking of positives above negatives.
```python
from sklearn.metrics import roc_curve, roc_auc_score
import matplotlib.pyplot as plt

fpr, tpr, thresholds = roc_curve(y_test, y_prob)
auc = roc_auc_score(y_test, y_prob)

plt.plot(fpr, tpr, label=f"AUC = {auc:.3f}")
plt.plot([0, 1], [0, 1], "k--", label="Random")
plt.xlabel("False Positive Rate"); plt.ylabel("True Positive Rate")
plt.legend(); plt.show()
```

## 4. Multi-Class Strategies
Logistic regression is natively binary; two standard strategies extend it (and other binary
classifiers, e.g., SVMs) to `K > 2` classes:
- **One-vs-Rest (OvR):** train `K` binary classifiers, each distinguishing one class from all
  others; predict the class whose classifier gives the highest score. scikit-learn's
  `LogisticRegression` handles this automatically (`multi_class="ovr"`, or the softmax-based
  multinomial solver by default in recent versions).
- **One-vs-One (OvO):** train `K*(K-1)/2` binary classifiers, one per pair of classes; predict by
  majority vote. More classifiers to train but each on a simpler, more balanced sub-problem;
  used by `SVC` by default for multi-class.

## 5. Handling Class Imbalance
When one class is much rarer than the other, a model optimized for overall accuracy tends to
under-predict the minority class.
- **Class weighting:** `class_weight="balanced"` reweights the loss inversely proportional to
  class frequency, penalizing minority-class mistakes more.
- **Resampling:** oversample the minority class or undersample the majority class before
  training.
- **Metric choice:** prefer precision/recall/F1 (or AUC) over raw accuracy, and consider
  precision-recall curves specifically for highly imbalanced problems.

```python
clf_balanced = LogisticRegression(class_weight="balanced", max_iter=1000)
clf_balanced.fit(X_train, y_train)
```

## 6. In-Class Exercise
Given a confusion matrix with TP=40, FP=10, FN=20, TN=130, compute precision, recall, and F1 by
hand, and state whether the model favors precision or recall in its current behavior.

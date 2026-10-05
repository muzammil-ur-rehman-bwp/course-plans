# Week 11: Classification and Model Evaluation Metrics

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain how k-nearest neighbors, decision trees and support vector machines classify a point.
2. Describe how the choice of `k` or of tree depth moves a model between underfitting and overfitting.
3. Read a confusion matrix, and compute accuracy, precision, recall and F1 from it.
4. Explain why accuracy alone can mislead on imbalanced data.
5. Draw and interpret an ROC curve and its AUC.

## 1. k-Nearest Neighbors

The k-nearest neighbors classifier is the easiest model to explain. To classify a new point, find the `k` training points closest to it, usually by Euclidean distance, and let them vote. The most common class among the neighbours wins.

There is no training phase in the usual sense. The model simply stores the data, which is why k-NN is called a lazy learner. All the work happens when you ask for a prediction.

To see exactly what happens, here is a small version written from scratch with NumPy, using the vectorized distance idea from Week 3.

```python
import numpy as np
from collections import Counter

def knn_predict(X_train, y_train, x_new, k=3):
    distances = np.linalg.norm(X_train - x_new, axis=1)
    nearest = np.argsort(distances)[:k]
    votes = Counter(y_train[nearest])
    return votes.most_common(1)[0][0]

X_toy = np.array([[1, 1], [1, 2], [2, 1], [6, 6], [7, 6], [6, 7]])
y_toy = np.array([0, 0, 0, 1, 1, 1])

print(knn_predict(X_toy, y_toy, np.array([2, 2])))   # 0
print(knn_predict(X_toy, y_toy, np.array([5, 5])))   # 1
```

The scikit-learn version does the same, with faster data structures for searching.

```python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline

data = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(
    data.data, data.target, test_size=0.25, random_state=42, stratify=data.target
)

knn = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=5))
knn.fit(X_train, y_train)
y_pred = knn.predict(X_test)
print("k-NN test accuracy:", round(knn.score(X_test, y_test), 3))
```

Because k-NN depends on distances, feature scaling is essential, and we use a pipeline as in Week 10. A feature measured in thousands would otherwise swamp one measured in fractions.

### 1.1 Choosing k

The number of neighbours controls the flexibility of the model.

1. Very small `k`, such as 1, lets every single training point decide, including noisy ones. The boundary becomes jagged and the model overfits.
2. Very large `k` averages over so many points that the boundary becomes smooth and blunt. The model underfits. In the extreme, `k` equal to the dataset size always predicts the majority class.

```python
print(" k   train  test")
for k in [1, 3, 5, 9, 15, 31, 75, 200]:
    m = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=k))
    m.fit(X_train, y_train)
    print(f"{k:3d}  {m.score(X_train, y_train):.3f}  {m.score(X_test, y_test):.3f}")
```

Look at the first row. With `k = 1` the training accuracy is exactly 1.0, since every point is its own nearest neighbour, but that says nothing about quality. The last row shows what happens when `k` is too big. In practice you choose `k` with cross-validation.

```python
from sklearn.model_selection import cross_val_score

for k in [1, 5, 15, 31]:
    m = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=k))
    scores = cross_val_score(m, X_train, y_train, cv=5)
    print(f"k={k:2d}  cv accuracy {scores.mean():.3f} +/- {scores.std():.3f}")
```

Cross-validation splits the training set into five parts, trains on four, and evaluates on the fifth, rotating through all five. The average is a steadier estimate than any single split, and the test set remains untouched.

## 2. Decision Trees and Support Vector Machines

### 2.1 Decision trees

A decision tree asks a sequence of yes or no questions about the features, such as "is the tumour radius greater than 15?", and each answer leads to another question or to a final class. Training chooses, at each step, the question that best separates the classes, measured by an impurity score such as Gini impurity.

Trees are easy to interpret, because you can print and read the rules. Their weakness is that an unrestricted tree can grow until every training point has its own leaf, which is overfitting.

```python
from sklearn.tree import DecisionTreeClassifier, export_text

tree = DecisionTreeClassifier(max_depth=3, random_state=0).fit(X_train, y_train)
print("tree test accuracy:", round(tree.score(X_test, y_test), 3))
print(export_text(tree, feature_names=list(data.feature_names)))
```

The printed rules can be read aloud as an explanation. Trees do not need feature scaling, since each question looks at a single feature at a time.

### 2.2 Support vector machines

A support vector machine (SVM) looks for the boundary that separates the classes with the widest possible margin, which is the empty street between them. The training points that touch the edges of that street are the support vectors, and only they determine the boundary.

If the classes cannot be separated by a straight line, a kernel lets the SVM work as if the data had been lifted into a higher dimensional space where a straight boundary exists. The RBF kernel is the usual default and produces smooth curved boundaries.

```python
from sklearn.svm import SVC

svm = make_pipeline(StandardScaler(), SVC(kernel="rbf", probability=True, random_state=0))
svm.fit(X_train, y_train)
print("SVM test accuracy:", round(svm.score(X_test, y_test), 3))
```

SVMs are sensitive to feature scale, so they also sit inside a pipeline. The setting `probability=True` makes the model produce probability estimates, at the cost of extra training time. We need these for the ROC curve later.

## 3. Evaluation Metrics

Accuracy, the fraction of correct predictions, is the first number everyone computes. It is also the easiest to misread. To understand why, we break the predictions into four groups. Consider the positive class to be the one we care about finding, such as the presence of a disease.

1. True positive (TP): predicted positive, actually positive.
2. True negative (TN): predicted negative, actually negative.
3. False positive (FP): predicted positive, actually negative.
4. False negative (FN): predicted negative, actually positive.

The metrics are formed from these counts.

1. Accuracy = (TP + TN) / (TP + TN + FP + FN). Use it when the classes are roughly balanced and both kinds of error cost about the same.
2. Precision = TP / (TP + FP). Of the items we called positive, what fraction really were? It matters when false alarms are costly, for example when flagging legitimate email as spam.
3. Recall = TP / (TP + FN). Of the truly positive items, what fraction did we find? It matters when missing a positive is costly, for example missing a disease.
4. F1 = 2 * Precision * Recall / (Precision + Recall). The harmonic mean of the two. It is high only when both are high.

```python
from sklearn.metrics import confusion_matrix, classification_report

y_pred = svm.predict(X_test)
cm = confusion_matrix(y_test, y_pred)
print(cm)
print(classification_report(y_test, y_pred, target_names=data.target_names))
```

In scikit-learn the confusion matrix has actual classes in rows and predicted classes in columns. Here class 0 is "malignant" and class 1 is "benign", so the top left cell counts malignant tumours correctly identified, and the bottom left cell counts benign predictions that were actually malignant. Read carefully which class you call "positive". In this dataset, a missed malignant tumour is the most serious error, so recall for the malignant class is the number to watch.

Compute the numbers by hand for the malignant class, and check them against the report.

```python
tp = cm[0, 0]
fn = cm[0, 1]
fp = cm[1, 0]
precision = tp / (tp + fp)
recall = tp / (tp + fn)
f1 = 2 * precision * recall / (precision + recall)
print(f"malignant: precision {precision:.3f}  recall {recall:.3f}  f1 {f1:.3f}")
```

### 3.1 Why accuracy can mislead

Suppose only 2 percent of transactions are fraudulent. A model that always answers "not fraud" achieves 98 percent accuracy and catches nothing. Let us build exactly this situation.

```python
from sklearn.datasets import make_classification
from sklearn.linear_model import LogisticRegression
from sklearn.dummy import DummyClassifier
from sklearn.metrics import accuracy_score, recall_score, precision_score

Xf, yf = make_classification(
    n_samples=5000, n_features=10, n_informative=4, weights=[0.98, 0.02],
    class_sep=2.5, flip_y=0.01, random_state=0
)
Xf_tr, Xf_te, yf_tr, yf_te = train_test_split(Xf, yf, test_size=0.3, random_state=0, stratify=yf)
print("fraud rate in test set:", yf_te.mean().round(3))

always_no = DummyClassifier(strategy="most_frequent").fit(Xf_tr, yf_tr)
lr = make_pipeline(StandardScaler(), LogisticRegression(max_iter=1000)).fit(Xf_tr, yf_tr)

for name, m in [("always 'not fraud'", always_no), ("logistic regression", lr)]:
    p = m.predict(Xf_te)
    print(f"{name:20s} accuracy {accuracy_score(yf_te, p):.3f}  "
          f"recall {recall_score(yf_te, p):.3f}  precision {precision_score(yf_te, p, zero_division=0):.3f}")
```

The dummy model has accuracy near 0.976 and recall of exactly zero. The logistic regression reaches about 0.98, which looks only marginally better, yet its recall is nearly 0.28 against zero. Accuracy hides that the first model is useless and the second at least finds some fraud. Even so, a recall of 0.28 is poor, which is a sign that more work is needed on features or model. On imbalanced problems report precision, recall and F1 for the minority class, along with the confusion matrix, and do not rely on accuracy.

### 3.2 The threshold is a choice

A classifier that outputs probabilities turns them into classes with a threshold, 0.5 by default. Lowering the threshold finds more positives, which increases recall, but also raises false alarms and lowers precision.

```python
probs = lr.predict_proba(Xf_te)[:, 1]
print("threshold  precision  recall")
for th in [0.5, 0.3, 0.2, 0.1, 0.05]:
    p = (probs >= th).astype(int)
    print(f"{th:9.2f}  {precision_score(yf_te, p, zero_division=0):9.3f}  {recall_score(yf_te, p):6.3f}")
```

Which threshold is right is a business or clinical decision, not a statistical one. It depends on what a missed fraud costs compared with a false alarm.

## 4. ROC Curves and AUC

The ROC curve describes performance across all thresholds at once. For each threshold it plots the true positive rate (which is recall) against the false positive rate, `FP / (FP + TN)`. A model that guesses randomly follows the diagonal. A perfect model goes straight up the left side and across the top.

The area under the curve (AUC) condenses the curve into a number. 1.0 is perfect and 0.5 is random guessing. An intuitive meaning: AUC is the probability that the model gives a randomly chosen positive example a higher score than a randomly chosen negative one.

```python
import matplotlib.pyplot as plt
from sklearn.metrics import roc_curve, roc_auc_score

fpr, tpr, thresholds = roc_curve(yf_te, probs)
auc = roc_auc_score(yf_te, probs)

plt.plot(fpr, tpr, color="black", label=f"logistic regression (AUC = {auc:.3f})")
plt.plot([0, 1], [0, 1], linestyle="--", color="gray", label="random guessing")
plt.xlabel("false positive rate")
plt.ylabel("true positive rate")
plt.title("ROC curve")
plt.legend()
plt.show()

print("AUC:", round(auc, 3))
```

AUC is insensitive to class imbalance in the sense that it does not depend on any single threshold, but on very imbalanced data a precision recall curve is often more informative, since the huge number of true negatives makes the false positive rate look small.

## 5. Worked Example: Comparing Three Classifiers

We train k-NN, a decision tree and an SVM on the same data, and compare them using several metrics, not just accuracy.

```python
from sklearn.metrics import f1_score

models = {
    "k-NN (k=5)": make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=5)),
    "Decision tree (depth 4)": DecisionTreeClassifier(max_depth=4, random_state=0),
    "SVM (RBF)": make_pipeline(StandardScaler(), SVC(probability=True, random_state=0)),
}

print(f"{'model':26s} {'acc':>6s} {'prec':>6s} {'recall':>6s} {'f1':>6s} {'auc':>6s}")
for name, m in models.items():
    m.fit(X_train, y_train)
    p = m.predict(X_test)
    s = m.predict_proba(X_test)[:, 1]
    print(f"{name:26s} {accuracy_score(y_test, p):6.3f} "
          f"{precision_score(y_test, p):6.3f} {recall_score(y_test, p):6.3f} "
          f"{f1_score(y_test, p):6.3f} {roc_auc_score(y_test, s):6.3f}")
```

Here the positive class is "benign" (label 1), so precision and recall describe benign predictions. For this medical problem you would normally care about recall for the malignant class, which you can obtain by passing `pos_label=0`.

```python
for name, m in models.items():
    p = m.predict(X_test)
    print(f"{name:26s} malignant recall: {recall_score(y_test, p, pos_label=0):.3f}")
```

With a test set of only 143 examples, differences of one or two percentage points are not meaningful. They correspond to a single patient. Always keep the size of the test set in mind when comparing models.

## 6. In-Class Exercise

Train k-NN, a decision tree and an SVM on the same dataset, produce a classification report for each, and discuss which metric matters most for the problem and why accuracy alone can mislead on imbalanced data.

Use the code above, and answer these questions in writing.

1. Which of the three models would you deploy for the breast cancer problem, and which metric did you use to decide?
2. For the fraud data, repeat the comparison of the three models. Which has the best recall for fraud? What price do you pay in precision?
3. How would your answer change if investigating each alert costs a lot of staff time?

```python
for name, m in {
    "k-NN": make_pipeline(StandardScaler(), KNeighborsClassifier(5)),
    "Tree": DecisionTreeClassifier(max_depth=4, random_state=0),
    "SVM": make_pipeline(StandardScaler(), SVC(random_state=0)),
}.items():
    m.fit(Xf_tr, yf_tr)
    p = m.predict(Xf_te)
    print(name, "recall", round(recall_score(yf_te, p), 3), "precision", round(precision_score(yf_te, p, zero_division=0), 3))
```

## 7. Common Mistakes

1. Quoting accuracy on a heavily imbalanced dataset.
2. Mixing up the rows and columns of the confusion matrix, or forgetting which class is positive.
3. Using k-NN or an SVM without scaling the features.
4. Choosing `k` or the tree depth by looking at the test score.
5. Treating small differences on a small test set as real.
6. Keeping the default 0.5 threshold without asking whether it suits the costs of the problem.

## 8. Summary

k-NN, decision trees and SVMs represent three different ideas: voting among neighbours, a sequence of rules, and a maximum margin boundary. Each has a setting that trades flexibility against overfitting. Evaluating them well needs more than accuracy. The confusion matrix, precision, recall, F1 and ROC curves describe different aspects of performance, and the right one depends on the cost of each kind of error.

## 9. Practice Problems

1. Compute by hand the precision, recall, F1 and accuracy for TP = 40, FP = 10, FN = 20, TN = 930. Then check with scikit-learn.
2. Plot the ROC curves of all three models on one set of axes.
3. Use `class_weight="balanced"` with logistic regression on the fraud data, and describe the effect on precision and recall.
4. Explain in your own words why a model with AUC 0.5 is no better than coin tossing, even if its accuracy on a given test set happens to be high.

## 10. Suggested Reading

1. The scikit-learn user guide, section on model evaluation.
2. Géron, Hands-On Machine Learning, the chapter on classification.

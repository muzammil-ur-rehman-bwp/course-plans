# Week 4 Lecture Plan — Introduction to Machine Learning
## Topic: Logistic Regression & Classification Metrics in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the sigmoid function, log-odds, and the logistic regression decision boundary.
   (*Understand*)
2. Apply precision, recall, F1, and ROC/AUC to evaluate a classifier beyond raw accuracy.
   (*Apply*)
3. Analyze multi-class strategies and class-imbalance handling to select an appropriate approach
   for a given dataset. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | The sigmoid function & decision boundary | From linear score to probability; threshold at 0.5 |
| 0:20–0:40 | Fitting logistic regression | `LogisticRegression`; interpreting coefficients as log-odds |
| 0:40–0:50 | Break | — |
| 0:50–1:20 | Precision, recall, F1, confusion matrix | Worked numeric example; when each metric matters |
| 1:20–1:40 | ROC curves and AUC | Threshold sweep; AUC as ranking quality |
| 1:40–2:00 | Multi-class & imbalance | One-vs-rest / one-vs-one; `class_weight`, resampling |

### Materials/Equipment
- Live-coding environment, scikit-learn; a binary dataset and an imbalanced dataset.

### Formative Check (in-class)
Exercise: given a confusion matrix, compute precision, recall, and F1 by hand, and identify
whether the classifier under- or over-predicts the positive class.

### Link to Lab/Assessment
Lab 4: logistic regression lab with a full classification report, ROC/AUC, and a class-imbalance
comparison.

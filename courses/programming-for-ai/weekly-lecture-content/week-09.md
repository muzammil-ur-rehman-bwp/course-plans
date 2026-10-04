# Week 9 — Lecture Content: Midterm + Intro to Machine Learning

## 1. Midterm Exam
Covers Weeks 1–8 (Python/NumPy/pandas, search, CSP/local search, probability/Naive Bayes). See
the Week 8 review materials for practice problems.

## 2. The Machine Learning Taxonomy
- **Supervised learning**: learn a mapping from inputs to known output labels (regression,
  classification). Example: predicting house prices from features.
- **Unsupervised learning**: find structure in data without labels (clustering, dimensionality
  reduction). Example: grouping customers by purchasing behavior.
- **Reinforcement learning** (overview only, not covered in depth this course): an agent learns
  by interacting with an environment and receiving rewards.

## 3. The ML Workflow
1. **Collect/load data** (pandas).
2. **Explore & clean** (EDA, Week 4 skills).
3. **Split** into training and test sets — the test set must never be used during training.
4. **Choose and train a model** (scikit-learn).
5. **Evaluate** on the test set with appropriate metrics.
6. **Iterate**: adjust features/model/hyperparameters and repeat.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```
`random_state` fixes the random seed so splits are reproducible — important for comparing models
fairly across experiments.

## 4. Why a Train/Test Split?
Evaluating a model on the same data it was trained on gives an overly optimistic (and
misleading) view of performance — the model may simply memorize training examples rather than
learn generalizable patterns. The held-out test set approximates performance on unseen data.

## 5. In-Class Exercise
Load a provided dataset, perform a train/test split, and describe (in words) which ML category —
supervised or unsupervised — would apply and why.

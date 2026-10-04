# Week 1 — Lecture Content: Introduction to Machine Learning

## 1. What Is Machine Learning?
Machine learning is the study of algorithms that improve their performance on a task from
experience (data), rather than following only explicitly programmed rules. A classic informal
definition (Mitchell, 1997): a program learns from experience `E` with respect to task `T` and
performance measure `P` if its performance at `T`, measured by `P`, improves with `E`.

## 2. The Learning Taxonomy
- **Supervised learning:** learn a mapping `f(x) -> y` from labeled examples `(x, y)`.
  - **Regression:** `y` is continuous (e.g., predicting house price).
  - **Classification:** `y` is categorical (e.g., spam vs. not spam).
- **Unsupervised learning:** find structure in unlabeled data `x` alone (e.g., clustering
  customers, reducing dimensionality for visualization).
- **Reinforcement learning:** an agent learns a policy by interacting with an environment and
  receiving rewards (mentioned for completeness; not a focus of this course).

This course is almost entirely about supervised learning (Weeks 2–10) and unsupervised learning
(Weeks 12–14), using classical/statistical methods rather than neural networks — a separate
*Introduction to Artificial Neural Networks* course covers that family in depth.

## 3. The ML Workflow
1. **Collect/load data.**
2. **Split** into training, validation, and test sets.
3. **Explore and preprocess** (handle missing values, encode categories, scale features — Week 11
   in depth).
4. **Train** one or more models on the training set.
5. **Evaluate** on the validation set; tune hyperparameters; iterate.
6. **Final evaluation** on the test set, touched only once, at the end.
7. **Deploy / report** the final model (Week 15).

```python
from sklearn.model_selection import train_test_split
import pandas as pd

df = pd.read_csv("housing.csv")
X = df.drop(columns=["target"])
y = df["target"]

# 80% train, 20% test — set aside a validation split or use cross-validation (Week 10)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

## 4. Why a Test Set? The Bias-Variance Tradeoff (Preview)
A model that is too simple **underfits**: it cannot capture the true pattern, giving high **bias**
and poor performance on both training and test data. A model that is too complex **overfits**: it
memorizes the training data (including its noise), giving low bias but high **variance** — it
performs very differently on new, unseen data. We can only detect overfitting by evaluating on
data the model never saw during training, which is exactly what the test set (and, during
development, a validation set or cross-validation) is for. Week 10 returns to this tradeoff with a
quantitative decomposition of expected test error.

## 5. Train/Validation/Test Splits
- **Training set:** used to fit model parameters.
- **Validation set (or cross-validation folds):** used to compare models/hyperparameters.
- **Test set:** touched exactly once, after all modeling decisions are final, to estimate
  real-world performance honestly.

```python
# A common pattern: split off a test set once, then use cross-validation on the remainder
# for model/hyperparameter selection (see Week 10) rather than a single fixed validation set.
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

**Important:** any preprocessing step that learns from data (e.g., scaling, imputation) must be
fit only on the training set and then applied to validation/test data — fitting it on the full
dataset before splitting leaks test information into the model and makes evaluation overly
optimistic. This idea — **data leakage** — recurs throughout the course.

## 6. In-Class Exercise
Classify five short scenarios (e.g., "predict tomorrow's temperature," "group customers by
purchasing behavior," "teach a robot to walk by trial and error") as supervised, unsupervised, or
reinforcement learning, and justify each in one sentence.

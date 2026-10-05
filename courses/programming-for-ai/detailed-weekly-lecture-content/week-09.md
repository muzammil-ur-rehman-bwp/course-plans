# Week 9: Midterm and Introduction to Machine Learning

## Learning Objectives

By the end of this lecture, you should be able to:

1. Complete the midterm exam covering Weeks 1 to 8 with a sound exam strategy.
2. Distinguish supervised, unsupervised and reinforcement learning, and give an example of each.
3. Describe the six step machine learning workflow.
4. Split data into training and test sets, and explain why the test set must be left alone until the end.
5. Recognise overfitting from the gap between training and test performance.

## 1. Midterm Exam

The midterm covers Weeks 1 to 8: Python, NumPy and pandas, search, constraint satisfaction and local search, and probability with Naive Bayes. The review checklist at the end of Week 8 lists the topics. The rest of this lecture is delivered after the exam, or in the remaining time of the session, and introduces the second half of the course.

A few suggestions for the exam itself.

1. Read the whole paper once before writing. Start with the question you are surest about, so that you collect marks early and settle down.
2. For tracing questions (BFS, DFS, A*), draw the frontier at each step in a small table. Marks are usually given for the process as well as for the final path.
3. For code questions, write the function signature and a short comment first, then fill in the body. A partly correct, clearly structured answer scores better than a dense one.
4. For probability questions, write the formula before substituting numbers.
5. Keep ten minutes at the end for checking units, signs and off-by-one errors.

## 2. The Machine Learning Taxonomy

In the first half of the course we wrote the rules ourselves. We told the search algorithm how to expand a node, and we told the CSP solver what the constraints were. Machine learning changes the direction. We give the computer examples and let it find the rules.

There are three main families.

1. Supervised learning. The training data contains inputs together with known outputs, called labels. The goal is to learn a mapping from inputs to outputs that works on new inputs.
   1. Regression: the output is a number. Example: predicting a house price from its area and location.
   2. Classification: the output is a category. Example: deciding whether an email is spam.
2. Unsupervised learning. The training data has no labels, and the goal is to find structure.
   1. Clustering: grouping similar items, for example grouping customers by buying behaviour.
   2. Dimensionality reduction: compressing many features into a few, for example to plot high-dimensional data.
3. Reinforcement learning. An agent interacts with an environment, takes actions, and receives rewards. It learns a policy that maximizes reward over time. This family is only an overview in this course. Game playing agents and robot controllers are typical examples.

A quick way to decide which family fits a problem: ask whether you have the answers for past examples. If yes, and you want to predict them for new cases, it is supervised. If you have only the raw data and want to explore it, it is unsupervised. If there is no dataset but an agent that can act and be rewarded, it is reinforcement learning.

### 2.1 Features and labels

The terminology is worth fixing now. Each example is a row. The features, often written `X`, are the input columns. The label, written `y`, is the quantity to predict. A feature matrix `X` has shape `(n_samples, n_features)`, and the label vector `y` has shape `(n_samples,)`.

```python
from sklearn.datasets import load_iris

iris = load_iris()
X, y = iris.data, iris.target
print("X shape:", X.shape)
print("y shape:", y.shape)
print("features:", iris.feature_names)
print("classes:", list(iris.target_names))
print("first row:", X[0], "->", iris.target_names[y[0]])
```

This is the famous iris dataset, with 150 flowers described by four measurements, each belonging to one of three species. It ships with scikit-learn, so no download is needed. Determining the species from the measurements is a supervised classification task.

## 3. The Machine Learning Workflow

A typical project has six steps.

1. Collect or load the data, usually with pandas.
2. Explore and clean it, using the EDA skills from Week 4.
3. Split it into a training set and a test set. The test set must not be used while the model is being built.
4. Choose a model and train it, using scikit-learn.
5. Evaluate it on the test set with a suitable metric.
6. Iterate. Adjust features, model type and hyperparameters, and repeat from step 4.

The split in step 3 is where beginners most often go wrong, so we look at it closely.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
print(X_train.shape, X_test.shape)
```

The argument `test_size=0.2` reserves twenty percent for testing. `random_state=42` fixes the seed, so that you and everyone else obtain the same split, and so that comparisons between models are fair. Any number works. The point is to fix it.

### 3.1 Stratified splitting

With a small or imbalanced dataset a random split may put very few examples of a rare class into the test set. The `stratify` option keeps the class proportions equal in both parts.

```python
import numpy as np

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
print("train class counts:", np.bincount(y_train))
print("test class counts: ", np.bincount(y_test))
```

Each class should now appear in the same proportion in both parts, which is ten of each class in the test set.

## 4. Why Split the Data?

A model that is evaluated on its own training data gives a flattering and misleading picture. It may simply have memorized the examples. The held-out test set stands in for the future: examples the model has never seen, which is what it will face in real use.

The following experiment makes the point. We use a deliberately flexible model, a decision tree with no depth limit, which can memorize anything. To make the effect clear, we add noise to the labels.

```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score

rng = np.random.default_rng(0)
y_noisy = y.copy()
flip = rng.random(len(y)) < 0.25                  # corrupt about a quarter of the labels
y_noisy[flip] = rng.integers(0, 3, flip.sum())

Xtr, Xte, ytr, yte = train_test_split(X, y_noisy, test_size=0.3, random_state=1)

tree = DecisionTreeClassifier(random_state=0).fit(Xtr, ytr)
print("training accuracy:", round(accuracy_score(ytr, tree.predict(Xtr)), 3))
print("test accuracy:    ", round(accuracy_score(yte, tree.predict(Xte)), 3))
```

The tree reaches perfect or near perfect accuracy on the training data, even though a quarter of the labels are nonsense. It has memorized the noise. On the test set the accuracy is far lower. This gap between training and test performance is the signature of overfitting.

The opposite problem is underfitting, where the model is too simple to capture the real pattern and does poorly on both sets. A depth limit gives us a dial between the two.

```python
print("depth  train  test")
for depth in [1, 2, 3, 5, 8, None]:
    t = DecisionTreeClassifier(max_depth=depth, random_state=0).fit(Xtr, ytr)
    tr = accuracy_score(ytr, t.predict(Xtr))
    te = accuracy_score(yte, t.predict(Xte))
    print(f"{str(depth):>5}  {tr:.3f}  {te:.3f}")
```

As depth grows, training accuracy climbs to 1.0, but test accuracy stops improving and may fall. The best model is not the one with the best training score.

### 4.1 One more rule: the test set is used once

Suppose you try twenty models and keep the one with the best test score. That score is now optimistic, because you effectively chose the model by looking at the test data. The honest approach is to carve out a third piece, a validation set, for choosing between models, and to touch the test set only once, at the very end. Later weeks introduce cross-validation, which does this more economically.

## 5. A First Complete Example

Putting the whole workflow together, with the iris data and a simple model.

```python
from sklearn.neighbors import KNeighborsClassifier

# 1 and 2. load, inspect
X, y = load_iris(return_X_y=True)

# 3. split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# 4. train
model = KNeighborsClassifier(n_neighbors=5).fit(X_train, y_train)

# 5. evaluate
pred = model.predict(X_test)
print("test accuracy:", round(accuracy_score(y_test, pred), 3))

# 6. iterate: try other settings
for k in [1, 3, 5, 9, 15]:
    m = KNeighborsClassifier(n_neighbors=k).fit(X_train, y_train)
    print(f"k={k:2d}  test accuracy {accuracy_score(y_test, m.predict(X_test)):.3f}")
```

Note that in step 6 we are comparing on the test set, which we warned against in section 4.1. For a short classroom demonstration this is acceptable, but in a real project the comparison in step 6 should use validation data.

## 6. A Common Trap: Data Leakage

Leakage means that information from the test set sneaks into training. The most common form is preprocessing the whole dataset before splitting. For example, if you standardize all rows using the global mean and standard deviation, the training data has been influenced by statistics of the test rows.

```python
from sklearn.preprocessing import StandardScaler

# wrong order: scaler sees the test data
scaler_wrong = StandardScaler().fit(X)

# right order: fit on training data only, then apply to both
scaler = StandardScaler().fit(X_train)
X_train_s = scaler.transform(X_train)
X_test_s = scaler.transform(X_test)
print("train mean (should be ~0):", X_train_s.mean(axis=0).round(2))
print("test mean (need not be 0):", X_test_s.mean(axis=0).round(2))
```

The test mean is not exactly zero, and that is correct. It reflects the fact that the scaler never saw the test data. Imputing missing values has the same rule: compute the fill values from the training portion only. Week 10 returns to this with pipelines.

## 7. In-Class Exercise

Load a provided dataset, perform a train and test split, and say in words which machine learning category, supervised or unsupervised, applies, and why.

Here is a worked version using the diabetes dataset, which comes with scikit-learn.

```python
from sklearn.datasets import load_diabetes

data = load_diabetes()
X, y = data.data, data.target
print("samples:", X.shape[0], " features:", X.shape[1])
print("target range:", y.min(), "to", y.max())

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
print("train:", X_train.shape, "test:", X_test.shape)
```

A good written answer is: "This is supervised learning, specifically regression. Each patient has ten measurements as features and a known number, a measure of disease progression after one year, as the label. We want to predict that number for new patients, so the output is continuous."

Now try the same for a dataset with no label.

```python
from sklearn.datasets import make_blobs

points, _ = make_blobs(n_samples=300, centers=3, random_state=0)
print(points.shape)
```

If we throw the second return value away, as the underscore does, we have points with no labels. Grouping them is an unsupervised problem, namely clustering. We study it in Week 12.

## 8. Common Mistakes

1. Evaluating on training data and reporting that as the performance.
2. Scaling, imputing or selecting features before the split.
3. Tuning on the test set and then reporting the test score as if it were an unbiased estimate.
4. Not fixing the random seed, so that results cannot be reproduced.
5. Reporting accuracy alone on an imbalanced dataset. We return to this in Week 11.

## 9. Summary

Machine learning replaces hand-written rules with rules learned from data. Supervised learning needs labelled examples, unsupervised learning looks for structure without them, and reinforcement learning learns from reward. The workflow is load, explore, split, train, evaluate and iterate. The most important discipline is to keep the test data out of every decision, because the gap between training and test performance is how overfitting is discovered.

## 10. Practice Problems

1. For each of these, name the learning type and the likely model output: predicting tomorrow's temperature, grouping news articles by topic, deciding whether a bank transaction is fraud, teaching a robot to walk.
2. Using the noisy iris data above, plot training and test accuracy against tree depth, and mark the depth you would choose.
3. Repeat the split with five different values of `random_state`, and record the spread of test accuracy. What does the spread tell you about trusting a single split?
4. Explain, with an example, how scaling before splitting can lead to leakage.

## 11. Suggested Reading

1. The scikit-learn user guide, section on cross-validation and evaluating estimator performance.
2. Géron, Hands-On Machine Learning, the first two chapters.

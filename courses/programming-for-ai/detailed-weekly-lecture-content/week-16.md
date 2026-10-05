# Week 16: Capstone, Course Review and AI Ethics

## Learning Objectives

By the end of this lecture, you should be able to:

1. Present a capstone project clearly: the problem, the method, the results and the lessons.
2. Connect the techniques of the course into one coherent picture.
3. Show with an experiment how an aggregate metric can hide poor performance for a subgroup.
4. Describe the main privacy and fairness concerns in applied machine learning.
5. Use AI coding assistants honestly and responsibly in academic work.

## 1. Capstone Presentations

Each student or team presents for five to seven minutes, followed by questions. The grading criteria are in `assignments/capstone-rubric.md`. A good presentation covers four things.

1. Problem framing. What is the question, who would care about the answer, and what kind of task is it: classification, regression, clustering or search? State clearly what the input and the output are.
2. Technique. Which methods from the course did you apply, and why those? A simple method that is well matched to the problem, with a clear explanation, is better than a complicated one that nobody can justify.
3. Results and evaluation. Which data did you use, how did you split it, which metrics did you choose, and how do you compare with a baseline? Report numbers with a measure of uncertainty if you can, for instance the cross-validation spread.
4. Lessons learned. What went wrong, what surprised you, and what would you do with another month?

### 1.1 Practical advice for the five minutes

1. Open with one sentence on the problem, and one on the result. The audience then knows where you are heading.
2. Use one figure per idea. A learning curve or a confusion matrix tells more than a paragraph of text.
3. Say what the baseline was. A model with 90 percent accuracy sounds good until the audience learns that always guessing the majority class gives 88 percent.
4. Be honest about limitations. Questioners respect a student who has already thought about the weaknesses.
5. Practise aloud with a timer. Most people speak faster than they think when nervous, but they also overrun when unrehearsed.

### 1.2 A skeleton to start from

The following script is a template for the technical core of a capstone. It follows the full workflow: loading, splitting, a baseline, cross-validated model comparison, tuning, a single look at the test set, and a saved record of the results. It runs as it stands on a built-in dataset, and you replace the loading step with your own data.

```python
import json
import numpy as np
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split, cross_val_score, GridSearchCV
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.dummy import DummyClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report

SEED = 0

# 1. load (replace with your own data)
data = load_wine()
X, y = data.data, data.target

# 2. hold out a test set that stays untouched until the end
X_dev, X_test, y_dev, y_test = train_test_split(
    X, y, test_size=0.2, random_state=SEED, stratify=y)

# 3. baseline, then candidate models, compared by cross-validation
candidates = {
    "baseline (most frequent)": DummyClassifier(strategy="most_frequent"),
    "logistic regression": make_pipeline(StandardScaler(), LogisticRegression(max_iter=2000)),
    "random forest": RandomForestClassifier(n_estimators=200, random_state=SEED),
}
results = {}
for name, model in candidates.items():
    scores = cross_val_score(model, X_dev, y_dev, cv=5)
    results[name] = (scores.mean(), scores.std())
    print(f"{name:26s} cv accuracy {scores.mean():.3f} +/- {scores.std():.3f}")

# 4. tune the chosen model
grid = GridSearchCV(
    make_pipeline(StandardScaler(), LogisticRegression(max_iter=2000)),
    {"logisticregression__C": [0.01, 0.1, 1, 10, 100]}, cv=5).fit(X_dev, y_dev)
print("best C:", grid.best_params_["logisticregression__C"])

# 5. one final evaluation on the test set
pred = grid.predict(X_test)
print(classification_report(y_test, pred, target_names=data.target_names))

# 6. keep a record of what you did, so the results can be reproduced
record = {
    "seed": SEED,
    "cv_results": {k: [round(v[0], 3), round(v[1], 3)] for k, v in results.items()},
    "best_params": {k: float(v) for k, v in grid.best_params_.items()},
    "test_accuracy": round(float((pred == y_test).mean()), 3),
}
print(json.dumps(record, indent=2))
```

The script contains nothing new. Every step is something you did in an earlier week, and the aim is that doing them in the right order becomes habit.

## 2. Course Review: The Big Picture

The course had a clear sequence.

1. Python and data skills (Weeks 1 to 4). Functions, data structures, NumPy for fast numerics, and pandas for real tables.
2. Classical AI (Weeks 5 to 8). Search, with BFS, DFS and A*. Constraint satisfaction and local search. Probability and Naive Bayes.
3. Statistical machine learning (Weeks 9 to 13). The workflow, regression, classification, clustering, and the evaluation tools to tell whether any of it works.
4. Neural networks (Weeks 14 and 15). The forward pass, backpropagation and training.
5. Capstone (Week 16). Using the techniques together in an original project.

Each phase used the tools of the previous one. NumPy from Week 3 is the machinery under the forward pass in Week 14. The train, validate and test discipline from Week 9 applies to every model since. The `Problem` interface of Week 5 and the `fit` and `predict` interface of scikit-learn are both examples of the same design idea: write algorithms against a small, stable interface so that components can be swapped.

### 2.1 A few connections worth remembering

1. A heuristic in A* and a prior in Bayes' rule are both ways of putting knowledge into an algorithm before it sees all the evidence.
2. Gradient descent appeared in Week 10 for regression and again in Week 15 for networks. It is one idea at different scales.
3. Overfitting appeared in Week 9 with trees, in Week 11 with k-NN, in Week 13 with polynomials and in Week 15 with networks. It is the central difficulty of learning from data, and the cure is always some combination of more data, a simpler model, regularization and honest validation.
4. Simulated annealing (Week 7) and stochastic gradient descent (Week 15) both use randomness productively, to escape traps and to cut cost.

A quick self-test. For each of the following problems, name a technique from the course and one metric or check you would use.

1. Finding the cheapest route through a warehouse for a robot.
2. Predicting the selling price of a used car.
3. Grouping support tickets without any labels.
4. Flagging fraudulent card transactions, where fraud is 0.2 percent of the data.
5. Recognising handwritten digits.

One sensible set of answers: A* with a distance heuristic, checked by comparing path cost with BFS on small cases; linear regression or a tree-based model, checked by cross-validated RMSE against a mean-predictor baseline; k-means after text vectorization and scaling, checked by silhouette score and by reading a sample of tickets from each cluster; logistic regression or a tree model, checked by precision and recall for the fraud class and the precision recall curve rather than accuracy; and a small neural network, checked by validation loss curves and test accuracy.

## 3. AI Ethics and Responsible Use

The techniques you have learned are powerful enough to affect people's lives, in decisions about loans, hiring, medical care, policing and what people see online. Responsible practice is part of the engineering, not an optional extra.

### 3.1 Bias and fairness

A model learns the patterns in its training data, including unwanted ones. If the data under-represents a group, or reflects past discrimination, the model will reproduce this and may amplify it. A model can also be unfair even without a sensitive attribute among its inputs, if other features act as proxies for it.

An important practical habit follows: report performance for each subgroup, not just in aggregate. A high overall accuracy can hide a very poor result for a minority.

The experiment below builds a synthetic population with two groups. Group B is small, and its relationship between features and outcome is different from that of group A. The model is trained on the pooled data.

```python
import numpy as np
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, recall_score

rng = np.random.default_rng(0)

def make_group(n, flip):
    X = rng.normal(size=(n, 2))
    signal = X[:, 0] - X[:, 1] if not flip else X[:, 0] + X[:, 1]
    y = (signal + rng.normal(0, 0.5, size=n) > 0).astype(int)
    return X, y

Xa, ya = make_group(2000, flip=False)    # large majority group
Xb, yb = make_group(100, flip=True)      # small group with a different pattern

X = np.vstack([Xa, Xb])
y = np.concatenate([ya, yb])
group = np.array(["A"] * len(ya) + ["B"] * len(yb))

X_tr, X_te, y_tr, y_te, g_tr, g_te = train_test_split(
    X, y, group, test_size=0.3, random_state=0, stratify=group)

model = LogisticRegression().fit(X_tr, y_tr)
pred = model.predict(X_te)

print(f"overall accuracy: {accuracy_score(y_te, pred):.3f}")
for g in ["A", "B"]:
    mask = g_te == g
    print(f"group {g}: n={mask.sum():4d}  accuracy {accuracy_score(y_te[mask], pred[mask]):.3f}  "
          f"recall {recall_score(y_te[mask], pred[mask]):.3f}")
```

The overall accuracy looks respectable, about 0.88, because group A dominates the data and scores about 0.90. But for group B the accuracy is about 0.57, not far from a coin toss, and its recall is below 0.5. The aggregate number hid the failure. If the model were used to make decisions about people in both groups, group B would be systematically badly served, and nobody looking at the headline metric would notice.

What can be done about it? There is no single fix, but common steps are to collect more representative data, to evaluate by group, to reconsider whether the features carry the same meaning for all groups, and, where appropriate, to train separate or weighted models.

```python
# one simple mitigation: give the small group more weight during training
weights = np.where(g_tr == "B", 8.0, 1.0)
model_w = LogisticRegression().fit(X_tr, y_tr, sample_weight=weights)
pred_w = model_w.predict(X_te)
for g in ["A", "B"]:
    mask = g_te == g
    print(f"weighted model, group {g}: accuracy {accuracy_score(y_te[mask], pred_w[mask]):.3f}")
```

The result is that group B improves only slightly, to about 0.60, while group A gets worse, falling to about 0.82. Weighting does not solve the problem here, since a single linear model simply cannot represent two opposing patterns. This is a useful lesson in its own right. A technical patch does not remove a structural problem, and the better answer in this case is to give the model the group information, or to build one model per group, and to ask whether using such information is appropriate and lawful in the application. These are questions for people and not just for code.

### 3.2 Data privacy

Training data often contains sensitive information. Questions to ask before using any dataset:

1. Where did the data come from, and did the people in it agree to this use?
2. Does it include personal identifiers, or fields that could identify someone in combination, such as postcode, birth date and gender?
3. How will it be stored, who can access it, and for how long?
4. Can the trained model leak information about its training data? Large models can sometimes reproduce parts of their training text.

Simple measures include removing direct identifiers, aggregating where possible, collecting only what you need, and keeping data and code under access control. Do not upload sensitive data to online services that you do not control.

### 3.3 Transparency and accountability

Be able to explain what a system does and what it cannot do. Report its limits, such as the data it was trained on and the groups it was not tested on. Decide who is accountable when it is wrong, and provide a way for affected people to appeal an automated decision. A model that is more accurate but impossible to question may be a worse choice than a slightly weaker one that can be explained.

### 3.4 Responsible use of AI coding assistants

AI coding assistants are now part of normal practice, and in this course they may be used according to the academic integrity policy in the syllabus. The principle is simple: the work you submit must be work you understand and can defend, and any use of the assistant must be stated honestly.

Good practice:

1. Use an assistant to explain an error message, to suggest an approach, or to review code you wrote, and then write and test the solution yourself.
2. Read every line that you keep. If you cannot explain a line, you do not yet own it.
3. Test generated code. Assistants produce code that looks plausible and is sometimes wrong, for instance by evaluating on training data, or by leaking test information through preprocessing, which are exactly the mistakes this course warned about.
4. Acknowledge the assistance in the way the policy requires.

Copying unexamined output as your own analysis defeats the learning goals of the capstone, and it will show in the questions session, since you will not be able to answer them.

The following short check is a useful habit. Whenever generated or borrowed code trains a model, search it for the points where data is split and where preprocessing is fitted.

```python
import re

generated_code = """
scaler = StandardScaler().fit(X)
X_scaled = scaler.transform(X)
X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2)
model.fit(X_train, y_train)
print(model.score(X_train, y_train))
"""

def review(code):
    issues = []
    lines = code.strip().splitlines()
    split_line = next((i for i, l in enumerate(lines) if "train_test_split" in l), None)
    fit_lines = [i for i, l in enumerate(lines) if re.search(r"Scaler\(\)\.fit|fit_transform\(X\)", l)]
    if split_line is not None and any(i < split_line for i in fit_lines):
        issues.append("preprocessing is fitted before the split (leakage)")
    if "random_state" not in code:
        issues.append("no random_state, so the split is not reproducible")
    if re.search(r"score\(X_train", code) and "score(X_test" not in code:
        issues.append("reports training score only")
    return issues

for problem in review(generated_code):
    print("-", problem)
```

This crude script is not a real code reviewer, but it shows the attitude: read the code looking for the specific mistakes you have learned about. All three of those issues are present in the example, and you now know how to recognise them.

## 4. Closing Discussion

Use these prompts for the final discussion. Each student should be ready to say one thing about their own project.

1. Where could your project go next? Options include more data, a different model family, better features, or a deployment as a small tool.
2. What would deployment involve? Think of monitoring for data drift, retraining, handling mistakes, and who is responsible.
3. Which of your results would you trust least, and what would you do to test it?
4. What was the hardest mistake to find, and how did you find it?

## 5. Summary

The course began with Python functions and ended with neural networks trained by backpropagation, and the thread through it was the discipline of formulating a problem clearly, choosing a method that matches it, and evaluating honestly. The ethical dimension is part of the same discipline. Always ask who the data represents, whom the model will affect, and what happens when it is wrong.

## 6. Final Practice Tasks

1. Take your capstone data and compute accuracy for every subgroup that makes sense, such as by region or by category. Does the overall metric hide anything?
2. Write a one page model card for your capstone model: its purpose, data, metrics, limits and the cases in which it should not be used.
3. Rerun your capstone notebook from a fresh environment and a fresh kernel. Does it reproduce your reported numbers exactly?

## 7. Suggested Reading

1. Barocas, Hardt and Narayanan, Fairness and Machine Learning, available online.
2. Mitchell et al., "Model Cards for Model Reporting", 2019.
3. The course's academic integrity policy and the capstone rubric.

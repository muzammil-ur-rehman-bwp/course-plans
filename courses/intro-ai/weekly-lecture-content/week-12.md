# Week 12 — Lecture Content: Introduction to Machine Learning (Survey)

**Scope note:** this week is a brief, honest survey. It exists so you can see how the
statistical/learning pillar of AI connects to everything before it in this course — it is not a
substitute for a full machine-learning course (that is a separate course in this curriculum,
and the applied *Programming for AI* course covers the scikit-learn-based workflow in depth).

## 1. Why Learning Matters for AI
Every agent so far in this course has relied on a hand-specified model: a `Problem` class, a
knowledge base, a STRIPS domain, or a set of CPTs. A **learning agent** instead improves its
performance element from experience, which matters whenever we cannot hand-specify rules for
every situation in advance (e.g., recognizing handwritten digits, or deciding which emails are
spam).

## 2. Supervised vs. Unsupervised Learning
- **Supervised learning**: learn a mapping from inputs to known output labels. Regression
  predicts a continuous value; classification predicts a discrete category. Example: predict
  whether an email is spam, given labeled examples.
- **Unsupervised learning**: find structure in data without labels, e.g., clustering similar
  examples together. Example: grouping customers by purchasing behavior with no predefined
  groups.

## 3. Decision Trees: A Worked Example
A decision tree classifies an example by asking a sequence of attribute-based questions. It is
built by recursively choosing the attribute that best splits the training examples, using
**entropy** and **information gain**.

Entropy of a set `S` with respect to a binary label: `H(S) = -p log2(p) - (1-p) log2(1-p)`,
where `p` is the fraction of positive examples.

```python
import math

def entropy(labels):
    n = len(labels)
    if n == 0:
        return 0.0
    p = sum(labels) / n  # fraction labeled True/1
    if p == 0 or p == 1:
        return 0.0
    return -p * math.log2(p) - (1 - p) * math.log2(1 - p)

def information_gain(examples, attribute, label_key):
    base_entropy = entropy([e[label_key] for e in examples])
    values = set(e[attribute] for e in examples)
    weighted_entropy = 0.0
    for v in values:
        subset = [e for e in examples if e[attribute] == v]
        weighted_entropy += (len(subset) / len(examples)) * entropy([e[label_key] for e in subset])
    return base_entropy - weighted_entropy
```

### Toy dataset: "Play Tennis?"
```python
examples = [
    {"outlook": "sunny",    "windy": False, "play": 0},
    {"outlook": "sunny",    "windy": True,  "play": 0},
    {"outlook": "overcast", "windy": False, "play": 1},
    {"outlook": "rainy",    "windy": False, "play": 1},
    {"outlook": "rainy",    "windy": True,  "play": 0},
]

for attribute in ["outlook", "windy"]:
    gain = information_gain(examples, attribute, "play")
    print(f"information gain for splitting on '{attribute}': {gain:.4f}")
```
Whichever attribute has the higher information gain is chosen as the root split; the same
procedure is then applied recursively to each resulting subset, which is exactly the ID3
algorithm — small and mechanical enough to trace fully by hand on a dataset this size.

## 4. Why a Train/Test Split Matters (Conceptual)
Evaluating a model on the same data it was trained on gives an overly optimistic view of its
performance — it may simply memorize examples rather than learn a generalizable pattern. A
held-out test set estimates how the model performs on data it has not seen. This course
introduces the idea conceptually; building a full train/evaluate pipeline with scikit-learn is
the applied-programming course's territory, not this one's.

## 5. Where ML Fits in the Course Map
A decision tree is, in a sense, a small search over possible trees, guided by an information-
theoretic heuristic (information gain) instead of a hand-designed heuristic like Week 4's
Manhattan distance — and once built, it behaves like a compact logical rule set reminiscent of
Week 6–7's Horn clauses. Machine learning did not replace search and logic; it is a different
way of *obtaining* the rules/models that earlier weeks assumed were hand-specified.

## 6. In-Class Exercise
Compute the information gain of `outlook` and `windy` on the toy dataset by hand, confirm which
one the tree should split on first, and sketch the resulting one-level tree.

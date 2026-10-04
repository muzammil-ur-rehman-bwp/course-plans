# Week 8 — Lecture Content: Probability & Uncertainty for AI

## 1. Probability Basics
- **Joint probability**: `P(A, B)` — probability of both A and B.
- **Conditional probability**: `P(A | B) = P(A, B) / P(B)`.
- **Independence**: A and B are independent if `P(A, B) = P(A) * P(B)`.

## 2. Bayes' Rule
```
P(A | B) = P(B | A) * P(A) / P(B)
```
This lets us "flip" a conditional probability — e.g., compute `P(class | features)` from
`P(features | class)` and prior class probabilities, which is exactly what Naive Bayes does.

## 3. Naive Bayes Classifier
Assumes features are conditionally independent given the class (the "naive" assumption):
```
P(class | f1, f2, ..., fn) ∝ P(class) * P(f1 | class) * P(f2 | class) * ... * P(fn | class)
```
```python
import numpy as np
from collections import defaultdict

class NaiveBayesTextClassifier:
    def fit(self, docs, labels):
        self.classes = set(labels)
        self.class_priors = {}
        self.word_counts = {c: defaultdict(int) for c in self.classes}
        self.class_totals = {c: 0 for c in self.classes}
        for doc, label in zip(docs, labels):
            self.class_totals[label] += 1
            for word in doc.split():
                self.word_counts[label][word] += 1
        n = len(labels)
        self.class_priors = {c: self.class_totals[c] / n for c in self.classes}
        self.vocab = {w for c in self.classes for w in self.word_counts[c]}

    def predict(self, doc):
        scores = {}
        for c in self.classes:
            total_words = sum(self.word_counts[c].values())
            score = np.log(self.class_priors[c])
            for word in doc.split():
                # Laplace (add-one) smoothing
                count = self.word_counts[c].get(word, 0) + 1
                score += np.log(count / (total_words + len(self.vocab)))
            scores[c] = score
        return max(scores, key=scores.get)
```
Laplace smoothing (`+1` in the numerator, `+|vocab|` in the denominator) avoids zero
probabilities for words unseen in training for a given class.

## 4. Why This Matters for AI
Naive Bayes is the simplest bridge from classical probability to statistical machine learning —
it previews the supervised learning formulation (features → label) that dominates Weeks 9–13.

## 5. Midterm Review
Review session covers: Python/NumPy/pandas fluency, search problem formulation, BFS/DFS/A*
tracing, CSP/local search, and basic probability/Bayes' rule — i.e., all of Weeks 1–8.

## 6. In-Class Exercise
Hand-compute `P(spam | "free money")` for a toy two-word vocabulary and a small labeled training
set, then verify against the `NaiveBayesTextClassifier` implementation above.

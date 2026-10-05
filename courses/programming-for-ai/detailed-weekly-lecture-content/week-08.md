# Week 8: Probability and Uncertainty for AI

## Learning Objectives

By the end of this lecture, you should be able to:

1. Use joint, conditional and marginal probability correctly, and test two events for independence.
2. State Bayes' rule and apply it to a diagnostic style problem.
3. Explain the naive independence assumption and why it still works reasonably well.
4. Implement a Naive Bayes text classifier from scratch, using log probabilities and Laplace smoothing.
5. Hand-compute a posterior for a small spam example and verify it against code.
6. Review the main ideas of Weeks 1 to 8 ahead of the midterm.

## 1. Why Probability?

The search algorithms of the past three weeks assumed that the world is fully known. Real problems are not like that. A spam filter does not know for certain whether a message is junk, a medical system cannot see the disease directly, and a robot's sensors are noisy. We still have to act, so we need a language for reasoning with degrees of belief. Probability is that language, and it is the foundation of machine learning.

## 2. Probability Basics

We write `P(A)` for the probability of event A, a number between 0 and 1.

1. Joint probability, `P(A, B)`: the probability that both A and B occur.
2. Marginal probability, `P(A)`: the probability of A on its own. It is obtained by summing the joint probability over the other variable, `P(A) = P(A, B) + P(A, not B)`.
3. Conditional probability, `P(A | B) = P(A, B) / P(B)`: the probability of A once we know B has occurred. It is only defined when `P(B) > 0`.
4. Independence: A and B are independent if `P(A, B) = P(A) P(B)`. Equivalently, `P(A | B) = P(A)`, so knowing B tells us nothing about A.

A joint table shows how these fit together. Suppose we observe 1000 emails, each labelled spam or not, and each marked as containing the word "free" or not.

```python
import numpy as np

# rows: spam, not spam.  columns: contains "free", does not contain "free"
counts = np.array([[240, 60],
                   [ 30, 670]])
total = counts.sum()
joint = counts / total

print("joint table:\n", joint)
p_spam = joint[0].sum()
p_free = joint[:, 0].sum()
p_free_given_spam = joint[0, 0] / p_spam
p_spam_given_free = joint[0, 0] / p_free

print("P(spam)         =", round(p_spam, 3))
print("P(free)         =", round(p_free, 3))
print("P(free | spam)  =", round(p_free_given_spam, 3))
print("P(spam | free)  =", round(p_spam_given_free, 3))
print("independent?    ", np.isclose(joint[0, 0], p_spam * p_free))
```

Look carefully at the two conditional probabilities. `P(free | spam)` is 0.8: most spam contains the word. `P(spam | free)` is about 0.89: most mail containing the word is spam. They are close here, but they are not the same thing, and mixing them up is one of the commonest errors in reasoning about probability. The two events are clearly not independent.

## 3. Bayes' Rule

From the definition of conditional probability, `P(A, B) = P(A | B) P(B) = P(B | A) P(A)`. Rearranging gives Bayes' rule.

```
P(A | B) = P(B | A) * P(A) / P(B)
```

The rule lets us flip a conditional probability. We often know how likely the evidence is given the cause, and we want the probability of the cause given the evidence. The terms have names: `P(A)` is the prior, `P(B | A)` the likelihood, and `P(A | B)` the posterior. The denominator `P(B)` is a normalizing constant, and can be computed as `P(B | A) P(A) + P(B | not A) P(not A)`.

### 3.1 A classic example

A disease affects 1 percent of a population. A test detects it in 95 percent of people who have it (sensitivity). It wrongly reports a positive in 5 percent of people who do not have it (false positive rate). A person tests positive. What is the probability that they have the disease?

```python
def posterior(prior, sensitivity, false_positive_rate):
    evidence = sensitivity * prior + false_positive_rate * (1 - prior)
    return sensitivity * prior / evidence

print(round(posterior(0.01, 0.95, 0.05), 3))   # 0.161
```

Most people expect an answer near 95 percent. The correct answer is about 16 percent. The reason is that healthy people are so much more numerous that their false positives outnumber the true positives. If you want to convince yourself, simulate it.

```python
rng = np.random.default_rng(0)
n = 1_000_000
sick = rng.random(n) < 0.01
positive = np.where(sick, rng.random(n) < 0.95, rng.random(n) < 0.05)
print("simulated P(sick | positive) =", round(sick[positive].mean(), 3))
```

The simulation agrees with the formula, and it is a good habit to check a probability result both ways. The example also shows why priors matter. Ignoring the base rate is called the base rate fallacy.

## 4. The Naive Bayes Classifier

Now apply Bayes' rule to classification. Given a document with words `f1, ..., fn`, we want the class with the highest posterior.

```
P(class | f1, ..., fn)  is proportional to  P(class) * P(f1, ..., fn | class)
```

The joint likelihood of all the words given the class is hard to estimate, since most word combinations never appear in any training set. The naive step is to assume that, once the class is known, the features are independent of each other.

```
P(class | f1, ..., fn)  is proportional to  P(class) * P(f1 | class) * P(f2 | class) * ... * P(fn | class)
```

This is clearly false for language. The words "New" and "York" are not independent. Even so, classification only needs the right class to receive the highest score, and the errors in the independence assumption often affect all classes alike. That is why Naive Bayes is a reasonable baseline for text, and a good first model to learn.

### 4.1 Two practical issues

1. Underflow. Multiplying many small probabilities produces numbers too small for a computer to hold. We add logarithms instead, since `log(a * b) = log(a) + log(b)`, and the class with the highest log score is still the best class.
2. Zero probabilities. If a word never appeared in spam during training, its estimated probability given spam is zero, and the whole product becomes zero no matter how much other evidence there is. Laplace (add one) smoothing fixes this. We add 1 to every word count, and add the vocabulary size to the denominator, so that no probability is zero and the probabilities still sum to 1.

```
P(word | class) = (count(word, class) + 1) / (total words in class + |vocabulary|)
```

### 4.2 Implementation

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
        return self

    def scores(self, doc):
        scores = {}
        for c in self.classes:
            total_words = sum(self.word_counts[c].values())
            score = np.log(self.class_priors[c])
            for word in doc.split():
                # Laplace (add-one) smoothing
                count = self.word_counts[c].get(word, 0) + 1
                score += np.log(count / (total_words + len(self.vocab)))
            scores[c] = score
        return scores

    def predict(self, doc):
        scores = self.scores(doc)
        return max(scores, key=scores.get)

    def predict_proba(self, doc):
        s = self.scores(doc)
        m = max(s.values())
        exps = {c: np.exp(v - m) for c, v in s.items()}   # subtract max for stability
        z = sum(exps.values())
        return {c: v / z for c, v in exps.items()}
```

We made three small changes from the minimal version: `fit` returns `self` so calls can be chained, the scoring is separated out in `scores`, and `predict_proba` turns log scores into normalized probabilities. Subtracting the maximum before exponentiating is a standard numerical trick, and it does not change the result after normalization.

### 4.3 Trying it out

```python
docs = [
    "free money now",
    "free prize money",
    "win money free",
    "meeting at noon",
    "project money report",
    "lunch at noon",
]
labels = ["spam", "spam", "spam", "ham", "ham", "ham"]

nb = NaiveBayesTextClassifier().fit(docs, labels)
print("vocabulary size:", len(nb.vocab))

for text in ["free money", "meeting noon", "project report money", "free lunch"]:
    probs = nb.predict_proba(text)
    print(f"{text!r:26} -> {nb.predict(text):4s}  P(spam) = {probs['spam']:.3f}")
```

## 5. Worked Example: Hand Computation

The in-class task is to compute `P(spam | "free money")` by hand and confirm it with the code. Here is the full calculation, so that you can check your own.

Training data above has 3 spam and 3 ham messages, so both priors are 0.5. The vocabulary has 11 distinct words: free, money, now, prize, win, meeting, at, noon, project, report, lunch.

1. Spam messages contain 9 words in total. The word "free" appears 3 times and "money" appears 3 times.
2. Ham messages contain 9 words in total. The word "free" appears 0 times and "money" appears once (in "project money report").
3. With smoothing, each denominator is 9 + 11 = 20.

Spam score: `0.5 * (3 + 1)/20 * (3 + 1)/20 = 0.5 * 0.2 * 0.2 = 0.02`.

Ham score: `0.5 * (0 + 1)/20 * (1 + 1)/20 = 0.5 * 0.05 * 0.1 = 0.0025`.

Normalize: `P(spam | "free money") = 0.02 / (0.02 + 0.0025) = 0.889`.

Now check with code. The first line should print 0.889, and the second compares with scikit-learn's `MultinomialNB`, which implements the same model with `alpha=1` for the smoothing.

```python
print(round(nb.predict_proba("free money")["spam"], 3))

from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB

vec = CountVectorizer()
X = vec.fit_transform(docs)
clf = MultinomialNB(alpha=1.0).fit(X, labels)
prob = clf.predict_proba(vec.transform(["free money"]))[0]
print(dict(zip(clf.classes_, prob.round(3))))
```

The numbers agree. Note that scikit-learn's vectorizer lowercases and drops one letter words, and neither matters for our example, but they could matter for real text.

### 5.1 Why smoothing matters

```python
def score_no_smoothing(model, text, cls):
    total = sum(model.word_counts[cls].values())
    p = model.class_priors[cls]
    for w in text.split():
        p *= model.word_counts[cls].get(w, 0) / total
    return p

print("ham score for 'free money' without smoothing :", score_no_smoothing(nb, "free money", "ham"))
print("spam score for 'free money' without smoothing:", score_no_smoothing(nb, "free money", "spam"))
```

The ham score is exactly zero, since "free" never appeared in a ham message. A single unseen word would then completely veto a class. With smoothing, the ham score is small but positive, and the evidence from other words still counts.

## 6. Why This Matters for AI

Naive Bayes is the simplest bridge from classical probability to statistical machine learning. It already contains the supervised learning pattern that dominates Weeks 9 to 13: learn parameters from labelled examples, then predict the label of unseen inputs. It also shows the main trade-off of modelling, which is that a simple but wrong assumption can still give a useful model, but only if we test it on data that was not used in training. We take that up next week.

## 7. Midterm Review

The midterm covers Weeks 1 to 8. Use this checklist.

1. Python, NumPy and pandas fluency.
   1. Write functions with default arguments and comprehensions.
   2. Predict the shape that results from a broadcast operation.
   3. Select, filter, group and merge a DataFrame, and handle missing values.
2. Search.
   1. Formulate a problem with its six components.
   2. Trace BFS and DFS on a small graph, and state their properties.
   3. Trace A*, define admissible and consistent, and justify a heuristic.
3. CSPs and local search.
   1. Formulate a CSP, and trace backtracking with forward checking.
   2. Explain local optima, restarts and the role of temperature in annealing.
4. Probability.
   1. Apply Bayes' rule to a numerical problem.
   2. Compute a Naive Bayes posterior with smoothing.

A practice question for each area is a good use of an evening. For example: "A heuristic is consistent. Is A* with it optimal, and can a node be expanded twice?" or "A test is 90 percent sensitive with a 10 percent false positive rate and the base rate is 2 percent. What is the probability of disease after a positive result?" The first answer is yes to optimality and no to re-expansion. The second is about 15.5 percent.

```python
print(round(posterior(0.02, 0.90, 0.10), 3))   # 0.155
```

## 8. In-Class Exercise

Hand-compute `P(spam | "free money")` for the toy two word vocabulary and a small training set, and verify against `NaiveBayesTextClassifier`. We did this above for the full six message set. Now repeat it with a message that contains an unseen word, such as "free cash".

1. Compute the spam and ham scores by hand. Remember that "cash" is not in the vocabulary of the training data. Should the vocabulary size in the denominator include it?
2. Check the code's answer.
3. Explain, in a sentence, what the model does with an unseen word.

```python
print(nb.predict_proba("free cash"))
```

For "free cash" the code gives 0.8 for spam. In our implementation an unseen word gets the count 1 in every class, and the vocabulary size does not change, so the word contributes the factor 1/(total words in class + 11). Here both classes have 9 words, so the factor is identical and the word has no effect on the comparison. If the classes had different totals, the unseen word would slightly favour the class with fewer words, which is a small quirk of this simple smoothing scheme.

## 9. Common Mistakes

1. Confusing `P(A | B)` with `P(B | A)`.
2. Forgetting the prior, and so falling into the base rate fallacy.
3. Multiplying probabilities directly for long documents and getting zero through underflow.
4. Leaving out smoothing.
5. Using the posterior's value as if it were well calibrated. Naive Bayes tends to produce probabilities close to 0 or 1 even when it is uncertain, since the independence assumption double counts evidence.

## 10. Summary

Probability gives us a coherent way to reason under uncertainty. Bayes' rule converts likelihoods into posteriors, and Naive Bayes applies it with a strong independence assumption that makes learning cheap. We saw that log probabilities and smoothing are what make it work in practice. This closes the first half of the course, and from next week we turn to the machine learning workflow itself.

## 11. Practice Problems

1. Extend the classifier with a method that returns the ten most spam-indicating words, ranked by the ratio of `P(word | spam)` to `P(word | ham)`.
2. Two independent tests for the same disease are both positive. Using the numbers in section 3.1, compute the posterior after both.
3. Show with a small example that two events can be pairwise independent yet not independent as a set of three.
4. Train the classifier on a bigger dataset of your own, such as the SMS spam collection, and measure the accuracy on a held-out portion.

## 12. Suggested Reading

1. Russell and Norvig, the chapters on probability and on probabilistic reasoning.
2. Jurafsky and Martin, Speech and Language Processing, the chapter on Naive Bayes and sentiment classification.

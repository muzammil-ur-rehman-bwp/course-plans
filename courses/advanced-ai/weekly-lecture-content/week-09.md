# Week 9 — Lecture Content: AI Safety and Alignment II

*(Midterm Exam covers Weeks 1–8; this content is delivered in the remainder of Week 9's session.)*

## 1. Reward Modeling
Rather than hand-specifying a reward function (and risking the kind of outer-alignment gap seen
in Week 8), **reward modeling** learns a reward function from human feedback — typically
pairwise preference comparisons ("output A is better than output B") rather than absolute
numeric scores, since humans are generally more reliable at relative comparisons than at
assigning consistent absolute numbers. A standard approach fits a model r̂(x) such that
preferences are explained by a logistic (Bradley-Terry-style) model:
```
P(A ≻ B) = σ( r̂(A) − r̂(B) ),     σ(z) = 1 / (1 + e^{-z})
```
trained by maximizing the log-likelihood of the observed preference data. The resulting r̂ is
then used as a reward signal (e.g., to train a policy via RL against it), replacing a
hand-specified proxy with a learned one that is intended to track human judgment more closely.

```python
import numpy as np

def fit_reward_model(features_a, features_b, preferred_is_a, lr=0.05, epochs=500):
    """features_a, features_b: arrays of shape (n_pairs, d) -- feature representations of each
    compared item. preferred_is_a: boolean array, True where item A was preferred.
    Fits a linear reward model r(x) = w . x via logistic regression on pairwise preferences."""
    d = features_a.shape[1]
    w = np.zeros(d)
    y = preferred_is_a.astype(float)
    for _ in range(epochs):
        diff = features_a - features_b
        z = diff @ w
        p = 1.0 / (1.0 + np.exp(-z))
        grad = diff.T @ (y - p) / len(y)
        w += lr * grad
    return w  # reward model: r(x) = w . x
```
**Limitation, stated plainly:** a reward model is only as good as its training data — it inherits
any blind spots, inconsistencies, or systematic biases present in the human labelers' judgments,
and it can be confidently wrong on inputs far from its training distribution, exactly the
distribution-shift concern raised by inner alignment in Week 8.

## 2. Scalable Oversight
As AI systems are applied to tasks where direct human evaluation of outputs is difficult or
infeasible (the output is too long, too technical, or the correct answer is simply not known to
the evaluator), a family of techniques aims to make human (or AI-assisted human) oversight
**scale** to such tasks. Two generically described approaches:
- **Debate**: two systems argue opposing positions in front of a human judge; the hope is that,
  because dishonesty or error is easier to attack than defend in an adversarial exchange, a
  judge without domain expertise can still identify the more honest position more reliably than
  by evaluating a single unopposed answer directly.
- **Iterated amplification**: break a hard evaluation task into smaller sub-tasks that are each
  easier to evaluate directly (possibly recursively, with AI assistance at each level), and
  combine the sub-evaluations into a judgment of the original, harder task.
Both approaches target the same underlying problem — evaluation difficulty, not merely task
difficulty — and both are active, unsettled research directions rather than solved techniques;
this course describes their general mechanism and motivation, not a specific implementation's
guaranteed effectiveness.

## 3. Interpretability as a Safety Tool
If a system's internal computation could be directly inspected and understood (Week 10's full
topic), oversight would no longer need to rely solely on observing external behavior — a
misaligned internal objective (an inner-alignment failure, Week 8) might in principle be
detectable by examining the model's internals directly, even on inputs where its behavior still
looks correct. This motivates treating interpretability not merely as a usability or debugging
feature but as a candidate **safety** tool in its own right — a theme developed fully next week.

## 4. Why None of This Is "Solved"
Each direction above narrows a specific gap without closing it: reward modeling narrows the
outer-alignment gap but is bounded by its training data's quality and coverage; scalable
oversight targets the evaluation-difficulty problem but its guarantees (if any) depend on
assumptions about the adversarial dynamics of debate or the decomposability of amplification that
are themselves active research questions; interpretability-as-safety is bounded by how much of a
model's actual computation current interpretability methods can faithfully recover (Week 10).
Presenting any of these as a finished solution to alignment would overstate the current state of
the research — the technically accurate framing is that each is a serious, actively studied
partial mitigation.

## 5. In-Class/Lab Exercise
Generate synthetic pairwise-preference data for a toy "item quality" task with a known
ground-truth linear reward function plus labeling noise; fit `fit_reward_model` and compare the
learned weight vector to the ground truth. Then intentionally introduce a labeler bias (flip
preferences for items with one specific feature) and observe how it distorts the learned reward
model — a concrete illustration of the stated limitation in §1.

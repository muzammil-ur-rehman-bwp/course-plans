# Lab Manual 1 — Empirical Risk vs. True Risk

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Demonstrate, by direct implementation, that zero empirical risk over an unrestricted hypothesis
class guarantees nothing about true risk.

## Setup
Create `lab01.ipynb`. Use NumPy only (no scikit-learn needed this week).

## Procedure
1. **Task A — Target concept:** implement `true_concept(x)` for a simple linear threshold target
   in $\mathbb{R}^2$, as in the lecture content.
2. **Task B — Memorizer:** implement the "all functions" memorizer hypothesis class (a lookup
   table over training points, with a fixed default prediction for unseen inputs).
3. **Task C — Measure the gap:** compute empirical risk (on the training set) and an estimate of
   true risk (on a large, freshly drawn test set) for the memorizer. Report both.
4. **Task D — Restricted class:** repeat Task C, but using `sklearn.linear_model.LogisticRegression`
   (a *restricted* hypothesis class) instead of the memorizer, on the same data.
5. **Task E — Reflection:** write 4–6 sentences comparing the two gaps and explaining, in terms of
   Week 1's ERM framework, why restricting $H$ changed the outcome.

## Expected Output
A notebook with Tasks A–E; Task C must show empirical risk at or near 0 and true risk near
chance; Task D must show both risks reasonably close together.

## Submission
Submit `lab01.ipynb` by the end of the lab session.

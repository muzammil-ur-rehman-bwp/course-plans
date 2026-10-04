# Lab Manual 8 — AdaBoost From Scratch: Training Error and Margins

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement AdaBoost from scratch and empirically observe the training-error bound and the
margin-distribution behavior that explains boosting's resistance to overfitting.

## Setup
Create `lab08.ipynb`. NumPy, Matplotlib.

## Procedure
1. **Task A — Decision stump:** implement the weighted decision-stump weak learner from the
   lecture content.
2. **Task B — AdaBoost loop:** implement the full AdaBoost training loop (weight updates, $\alpha_t$
   computation) for $T=40$ rounds.
3. **Task C — Training error curve:** plot training error vs. round; confirm it reaches (or
   nears) zero well before round 40.
4. **Task D — Margin distribution:** plot the *distribution* (e.g., a histogram or CDF) of
   training-point margins at rounds 5, 20, and 40; confirm margins keep shifting rightward
   (increasing) even after training error is flat.
5. **Task E — Bagging comparison:** fit `sklearn.ensemble.BaggingClassifier` and your AdaBoost on
   the same data; compare test-set accuracy and discuss, referencing Week 8's variance-reduction
   vs. margin-theory arguments, why each method controls overfitting differently.

## Expected Output
A notebook with Tasks A–E, including the training-error curve and margin-distribution plots.

## Submission
Submit `lab08.ipynb` by the end of the lab session.

# Lab Manual 10 — Rademacher Complexity and Random-Label Fitting

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Empirically estimate a Rademacher-complexity-flavored quantity for a small hypothesis class, and
fit random labels with a small network to compare against normal-label performance.

## Setup
Create `lab10.ipynb`. Start from the lecture's `rademacher_estimate` function and MLP setup.

## Procedure
1. **Task A — Rademacher estimate vs. sample size:** compute the empirical Rademacher complexity
   estimate for the linear class on $X$ of sizes $m\in\{10,20,50,100\}$ (same dimensionality
   $d=5$); plot the estimate vs. $m$ and report the trend (should shrink as $m$ grows, roughly
   like $1/\sqrt m$, for a fixed-norm linear class).
2. **Task B — Random vs. real labels:** reproduce the lecture's MLP experiment; report train and
   test accuracy for both label conditions in a table.
3. **Task C — Capacity vs. data size:** repeat Task B's random-label run with training set sizes
   $\{50, 100, 200, 500\}$; report the smallest training set size at which the network **fails**
   to reach near-100% training accuracy on random labels, if any, within a fixed training budget.
4. **Task D — Margin check:** for the Task B real-labels run, compute the mean classification
   margin (predicted logit magnitude, signed correctly, averaged over training points) and
   compare it to the random-labels run's mean margin; report which is larger and connect this to
   the lecture's margin-based generalization argument.

## Expected Output
A notebook with Tasks A–D, including Task A's plot and Task B's results table.

## Submission
Submit `lab10.ipynb` by the end of the lab session.

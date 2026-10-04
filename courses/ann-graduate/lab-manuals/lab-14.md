# Lab Manual 14 — A Toy Binned Mutual-Information Estimate

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Estimate toy, binned mutual-information-like quantities between a small network's hidden layers
and its input/output across training epochs, with explicit discussion of the estimator's
limitations.

## Setup
Create `lab14.ipynb`. Start from the lecture's `binned_mi_estimate` function.

## Procedure
1. **Task A — Fitting vs. compression curves:** reproduce the lecture's epoch sweep, computing
   $I(T_1;Y)$ (fitting-flavored) for a hidden unit across training; additionally compute a
   binned estimate of $I(T_1;X_1)$ (compression-flavored, using one scalar input feature) across
   the same training run; plot both vs. epoch on one chart.
2. **Task B — Activation function comparison:** repeat Task A with a `tanh` hidden layer and
   with a `ReLU` hidden layer (same architecture otherwise); report whether the qualitative
   shape of the two curves (fitting and compression) differs between the two activations.
3. **Task C — Estimator sensitivity:** repeat Task A's tanh run with `n_bins` $\in\{3, 10, 30,
   100\}$; report how much the final-epoch $I(T_1;Y)$ estimate changes across bin counts.
4. **Task D — Discussion:** write 3–4 sentences stating (i) what pattern, if any, Task B shows
   across activations, and (ii) how Task C's sensitivity to `n_bins` should affect how confidently
   any such pattern is reported.

## Expected Output
A notebook with Tasks A–D, including Task A/B's plots and Task C's reported sensitivity.

## Submission
Submit `lab14.ipynb` by the end of the lab session.

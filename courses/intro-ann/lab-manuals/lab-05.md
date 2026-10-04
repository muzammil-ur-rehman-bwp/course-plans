# Lab Manual 5 — Loss Functions and the Saturation Problem

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement MSE, binary cross-entropy, and categorical cross-entropy; demonstrate the vanishing-
gradient problem MSE has with a saturating sigmoid output, and cross-entropy's fix.

## Setup
Create `lab05.ipynb`.

## Procedure
1. **Task A — Implementations:** implement `mse`, `binary_cross_entropy`, and
   `categorical_cross_entropy` as shown in lecture, including numerically safe clipping.
2. **Task B — Loss values:** for $y=1$ and $\hat y$ ranging over `np.linspace(0.001, 0.999, 50)`,
   plot $L_{\text{MSE}}$ and $L_{\text{BCE}}$ as two curves on one figure.
3. **Task C — Gradient comparison:** for the same range of $\hat y$, compute
   $\partial L/\partial z$ for both losses (treating $\hat y = \sigma(z)$) and plot both gradient
   curves vs. $\hat y$. Confirm visually that the MSE gradient curve flattens near $\hat y=0$
   while the BCE gradient curve does not.
4. **Task D — Categorical check:** for a 4-class one-hot label and a softmax output vector of
   your choosing, compute `categorical_cross_entropy` by hand and confirm it matches your code's
   output, and confirm it equals $-\log(\hat y_{k^*})$ directly.

## Expected Output
A notebook with Tasks A–D, including the two comparison plots from Tasks B and C.

## Submission
Submit `lab05.ipynb` by the end of the lab session.

# Lab Manual 12 — Computing a Small-Scale Neural Tangent Kernel

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Numerically compute an NTK-flavored kernel for a simple one-hidden-layer network at
initialization, and compare kernel-regression predictions against actually training the network.

## Setup
Create `lab12.ipynb`. Start from the lecture's `OneHiddenLayer`/`ntk_entry` code.

## Procedure
1. **Task A — Kernel computation:** compute the empirical NTK matrix $K$ for widths
   $\{20, 200, 2000\}$ on the same fixed set of 6 points; report how much $K$ changes across
   widths (e.g., Frobenius norm of the difference between consecutive widths' $K$).
2. **Task B — Kernel regression vs. trained network, on training points:** for width 2000,
   compare kernel-regression predictions (using $K$) to the actually-trained network's
   predictions on the 6 training points; report the max absolute difference.
3. **Task C — Off-training-point comparison:** add 4 new test points; compute kernel-regression
   predictions for them (using the cross-kernel between test and train points) and compare to the
   trained network's predictions on the same 4 points, for widths $\{20, 200, 2000\}$; report how
   the agreement changes with width.
4. **Task D — Discussion:** in 3–4 sentences, connect Task C's width-dependent agreement (or
   disagreement) to the lecture's claim that the NTK approximation improves as width grows.

## Expected Output
A notebook with Tasks A–D, including Task A's reported norms and Task C's comparison table.

## Submission
Submit `lab12.ipynb` by the end of the lab session.

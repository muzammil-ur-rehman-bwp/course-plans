# Lab Manual 6 — Hessian Eigenspectrum and Newton's Method Near a Saddle

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Compute the Hessian eigenvalue spectrum of a small network's loss at a found critical point, and
compare gradient descent's convergence near a saddle to Newton's method's on a tractable example.

## Setup
Create `lab06.ipynb`. `torch.autograd.functional.hessian` is permitted for Task B onward.

## Procedure
1. **Task A — Canonical saddle:** reproduce the lecture's $f(x,y)=x^2-y^2$ example: run gradient
   descent and Newton's method from the same near-saddle starting point, plot both trajectories
   on one 2D contour plot of $f$.
2. **Task B — A small network's Hessian:** build a 2-parameter toy network/loss (e.g.,
   $L(w_1,w_2) = (w_1 w_2 - 1)^2$, which has a non-trivial critical-point structure) and use
   `torch.autograd.functional.hessian` to compute its Hessian at a few candidate critical points
   found by running gradient descent to convergence; classify each via eigenvalue signs.
3. **Task C — Scaling up:** repeat Task B's Hessian computation for a small (e.g., 50-parameter)
   one-hidden-layer network trained briefly on a toy regression task; report the fraction of
   positive vs. negative eigenvalues at the point training stopped at.
4. **Task D — Discussion:** relate Task C's observed eigenvalue sign mixture to the lecture's
   high-dimensional saddle-dominance argument, in 3–4 sentences.

## Expected Output
A notebook with Tasks A–D, including the Task A contour plot and Task C's eigenvalue sign
histogram or count.

## Submission
Submit `lab06.ipynb` by the end of the lab session.

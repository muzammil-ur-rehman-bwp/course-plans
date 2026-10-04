# Lab Manual 6 — Gradient Descent Variants

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Implement and compare batch, stochastic, and mini-batch gradient descent on a simple 2D loss
surface and on a linear regression problem.

## Setup
Create `lab06.ipynb`.

## Procedure
1. **Task A — 1D convergence:** implement `gradient_descent` as shown in lecture; run it on
   $L(\theta)=(\theta-3)^2$ for learning rates $0.05, 0.5, 1.1$; plot $\theta$ vs. iteration for
   all three on one figure and label which converge/diverge.
2. **Task B — Linear regression setup:** generate a synthetic linear regression dataset
   ($y = 3x + 2 + \text{noise}$, 200 points); define the MSE loss and its gradient with respect to
   the slope/intercept parameters.
3. **Task C — Three variants:** implement batch, stochastic (batch size 1), and mini-batch
   (batch size 32) gradient descent for this regression problem, each run for the same number of
   epochs; plot training loss vs. epoch for all three on one figure.
4. **Task D — Shuffling ablation:** re-run mini-batch gradient descent with shuffling disabled
   (same batch order every epoch, constructed by sorting the data by `x` first); compare its loss
   curve to Task C's shuffled version and explain the difference in a markdown cell.

## Expected Output
A notebook with Tasks A–D, including three required plots.

## Submission
Submit `lab06.ipynb` by the end of the lab session.

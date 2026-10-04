# Lab Manual 7 — Verifying Backpropagation with Finite Differences

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Reproduce the lecture's numeric backpropagation example in NumPy, and verify every analytic
gradient against a numerical (finite-difference) approximation.

## Setup
Create `lab07.ipynb`.

## Procedure
1. **Task A — Forward pass:** implement the forward pass for the lecture's exact network
   ($W^{(1)}, b^{(1)}, W^{(2)}, b^{(2)}$, $x$, $y$ given in lecture) and confirm your computed
   $\hat y$ matches the lecture's value (0.7051) to 4 decimal places.
2. **Task B — Analytic backward pass:** implement the backward pass exactly as derived
   ($\delta^{(2)}, \partial L/\partial W^{(2)}, \partial L/\partial b^{(2)}, \delta^{(1)},
   \partial L/\partial W^{(1)}, \partial L/\partial b^{(1)}$) and confirm every value matches the
   lecture's worked example to 3 decimal places.
3. **Task C — Finite-difference check:** for each scalar parameter $\theta$ in $W^{(1)}, b^{(1)},
   W^{(2)}, b^{(2)}$, compute the numerical gradient
   $\frac{L(\theta+\epsilon) - L(\theta-\epsilon)}{2\epsilon}$ with $\epsilon = 10^{-5}$, and
   confirm it matches your analytic gradient from Task B to at least 4 decimal places for every
   parameter.
4. **Task D — Sensitivity:** repeat Task C with $\epsilon = 10^{-2}$ and $\epsilon = 10^{-8}$;
   report how the match quality changes and explain briefly (floating-point truncation error vs.
   finite-difference approximation error).

## Expected Output
A notebook with Tasks A–D; Task C must report, for every parameter, both the analytic and
numerical gradient side by side with their absolute difference.

## Submission
Submit `lab07.ipynb` by the end of the lab session.

# Lab Manual 1 — Prerequisite Refresher: Forward/Backward Pass From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Confirm (or restore) fluency with the from-scratch forward and backward pass of a small MLP,
checked against finite differences, before this course builds theory on top of it.

## Setup
1. Create a course virtual environment: `python -m venv venv && pip install numpy matplotlib torch`.
2. Create `lab01.ipynb`.

## Procedure
1. **Task A — Forward pass:** implement `forward(x, W1, b1, W2, b2)` for a 2-input, 3-hidden
   (tanh), 1-output (sigmoid) network. Compute the output for 3 arbitrary input vectors.
2. **Task B — Backward pass:** implement the backward pass by hand (using the recursive
   $\delta$ rule from the prerequisite course) for binary cross-entropy loss, returning gradients
   for every parameter.
3. **Task C — Finite-difference check:** for every scalar parameter, compute the numerical
   gradient with $\epsilon=10^{-5}$ and confirm it matches Task B's analytic gradient to at least
   4 decimal places.
4. **Task D — Self-diagnosis:** if any gradient fails to match, debug it before proceeding; this
   lab's passing state is the baseline every later lab assumes.

## Expected Output
A notebook with Tasks A–D; Task C must report, for every parameter, the analytic and numerical
gradient side by side with their absolute difference.

## Submission
Submit `lab01.ipynb` by the end of the lab session.

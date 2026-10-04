# Lab Manual 3 — Activation Functions and Their Derivatives

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Implement and visualize sigmoid, tanh, ReLU, Leaky ReLU, and softmax along with their derivatives.

## Setup
Create `lab03.ipynb`.

## Procedure
1. **Task A — Implementations:** implement `sigmoid`, `sigmoid_grad`, `tanh`, `tanh_grad`, `relu`,
   `relu_grad`, `leaky_relu`, `leaky_relu_grad`, and `softmax` as shown in lecture.
2. **Task B — Plots:** on a shared $z \in [-6,6]$ grid, plot all four activation functions on one
   figure and their derivatives on a second figure.
3. **Task C — Saturation analysis:** for sigmoid and tanh, print the derivative value at
   $z = -6, -2, 0, 2, 6$; identify, numerically, the range where the derivative drops below 0.01.
4. **Task D — Softmax sanity check:** for a logit vector of your choice (length ≥ 3), confirm your
   `softmax` output sums to 1.0 and is numerically stable for a vector containing a large value
   (e.g., 1000) by comparing against a naive (non-max-shifted) implementation.

## Expected Output
A notebook with Tasks A–D, including the two comparison plots.

## Submission
Submit `lab03.ipynb` by the end of the lab session.

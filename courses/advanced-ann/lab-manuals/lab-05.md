# Lab Manual 5 — Feature Learning vs. Kernel Regression Across Width

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Run the course's central empirical comparison — kernel-regression predictions against the
empirical NTK at initialization vs. actually training the network — across a range of widths, and
characterize how the gap between them changes with width.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab05.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `MLP`, `ntk_matrix`, `kernel_regression_predict`, and
   `experiment` exactly as in the Week 5 lecture content.
2. **Task B — Width sweep:** run `experiment(width)` for at least 5 widths spanning at least two
   orders of magnitude, recording kernel-regression test MSE, trained-network test MSE, and kernel
   drift at each.
3. **Task C — Plotting:** plot all three quantities against width (log-x axis) on appropriately
   scaled axes, and identify the width range where the trained-network/kernel-regression gap is
   largest.
4. **Task D — Representation probe (mini-challenge):** at the smallest and largest widths tried,
   train a simple linear probe (e.g., `sklearn.linear_model.Ridge` or a manually implemented
   least-squares fit) on the trained network's hidden-layer activations (before and after
   training) to predict the target, and report whether probe accuracy improves more after
   training at the smaller or the larger width — connecting to Week 5 §3's representation-
   alignment signature.

## Expected Output
A notebook with four clearly labeled sections (A–D), including the required plots and the Task D
comparison.

## Submission
Export/submit `lab05.ipynb` via the course submission system by the end of the lab session.

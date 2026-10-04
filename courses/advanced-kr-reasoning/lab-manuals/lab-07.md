# Lab Manual 7 — Differentiable Soft-Logic Loss (PyTorch)

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Implement and compare t-norms/t-conorms, then build and train a differentiable soft-logic loss.

## Setup
1. Reuse your Week 1 environment (PyTorch verified in Lab 1).
2. Create `lab07.ipynb`.

## Procedure
1. **Task A — t-norm/t-conorm implementation:** implement `product_tnorm`, `godel_tnorm`,
   `luk_tnorm` and their t-conorm/negation counterparts exactly as in the Week 7 lecture content.
2. **Task B — Gradient comparison:** at `a=0.9, b=0.2`, compute each t-norm's value and its
   gradient w.r.t. `a` (symbolically or via `torch.autograd`); confirm product's gradient is
   nonzero (0.2) while Gödel's is zero.
3. **Task C — Soft-logic loss:** implement `soft_implies` and `constraint_loss` exactly as in the
   lecture content; run the 50-step gradient-descent training loop and plot loss vs. step,
   confirming it decreases.
4. **Task D — Mini-challenge:** repeat Task C's training loop using a Gödel-t-conorm-based
   implication instead of the Łukasiewicz one; plot both loss curves on the same axes and explain,
   in 2–3 sentences, the training-speed difference observed, connecting it to Task B's gradient
   finding.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the two required plots.

## Submission
Export/submit `lab07.ipynb` via the course submission system by the end of the lab session.

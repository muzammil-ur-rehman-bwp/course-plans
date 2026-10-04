# Lab Manual 5 — Batch Normalization and Layer Normalization From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement BatchNorm's forward and backward pass from scratch, implement LayerNorm, and compare
their behavior as batch size shrinks toward 1.

## Setup
Create `lab05.ipynb`. Start from the lecture's `BatchNorm1D` class.

## Procedure
1. **Task A — Gradient check:** reproduce the lecture's finite-difference gradient check for
   `BatchNorm1D.backward`, confirming `max |analytic - numerical| dx` is below $10^{-5}$.
2. **Task B — Train/eval correctness:** train a `BatchNorm1D` layer (as part of a small 2-layer
   network) on a synthetic classification task for 200 steps; after training, run the same input
   batch through in both `training=True` and `training=False` mode and confirm the outputs
   differ (eval mode should use running statistics, not this batch's statistics).
3. **Task C — Implement LayerNorm:** implement a `LayerNorm1D` class (normalize across features
   per example); confirm it gives identical output whether called with batch size 32 or batch
   size 1 on the same individual example (BatchNorm should **not** have this property).
4. **Task D — Batch-size sweep:** for batch sizes $\{32, 8, 2, 1\}$, compare BatchNorm's and
   LayerNorm's output statistics (mean/variance across the batch) and report at what batch size
   BatchNorm's normalization becomes visibly unstable or degenerate.

## Expected Output
A notebook with Tasks A–D; Task D must include a short table of observed statistics per batch
size for both normalization layers.

## Submission
Submit `lab05.ipynb` by the end of the lab session.

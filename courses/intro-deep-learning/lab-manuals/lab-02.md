# Lab Manual 2 — Initialization, Batch Normalization, and Dropout

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Compare training behavior of a deep MLP under different initialization schemes, with and without
batch normalization, and with and without dropout.

## Setup
Create `lab02.ipynb`. Use the provided deep MLP (6+ hidden layers) and `lab02_data` dataset.

## Procedure
1. **Task A — Initialization comparison:** train the deep MLP three ways: default PyTorch
   initialization, all-zero initialization, and Xavier/He initialization (matched to the
   activation used); plot training loss curves for all three on one chart.
2. **Task B — Batch normalization:** add `nn.BatchNorm1d` after each hidden layer; retrain and
   compare the loss curve and training stability (e.g., sensitivity to a higher learning rate)
   against Task A's best configuration.
3. **Task C — Dropout:** add `nn.Dropout(p=0.5)` to the Task B model; compare training vs.
   validation loss curves with and without dropout, to see both its regularizing effect and its
   effect on apparent training loss.
4. **Task D — Gradient inspection:** for the deepest configuration, log and plot the gradient norm
   at each layer for one batch, with and without batch normalization, and relate the pattern to
   vanishing/exploding gradients.

## Expected Output
A notebook with Tasks A–D, all required plots, and a brief written interpretation of each
comparison (2–3 sentences per task).

## Submission
Submit `lab02.ipynb` by the end of the lab session.

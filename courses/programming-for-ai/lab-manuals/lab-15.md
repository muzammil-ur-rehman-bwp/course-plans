# Lab Manual 15 — Training a Neural Network with PyTorch/Keras

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Train a small feedforward network end-to-end on an image classification task.

## Setup
`pip install torch torchvision` (or `tensorflow`); create `lab15.ipynb`.

## Procedure
1. **Task A — Data loading:** load a small image dataset (e.g., MNIST or a subset) using the
   framework's standard data-loading utilities.
2. **Task B — Model definition:** define a small feedforward network (as shown in lecture).
3. **Task C — Training loop:** train for a modest number of epochs; record training and
   validation loss each epoch.
4. **Task D — Evaluation & plotting:** plot training/validation loss curves; report final test
   accuracy.
5. **Task E — Reflection:** based on the loss curves, state whether the model is underfitting,
   overfitting, or reasonably well-fit, and propose one concrete change to improve it.

## Expected Output
A notebook with Tasks A–E.

## Submission
Submit `lab15.ipynb` by the end of the lab session.

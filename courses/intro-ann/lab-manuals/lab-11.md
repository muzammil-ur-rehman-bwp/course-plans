# Lab Manual 11 — Regularization From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Add L2 regularization and dropout to the from-scratch network; compare training/validation
curves with and without regularization.

## Setup
Create `lab11.ipynb`, reusing the Week 8/10 `NeuralNetwork` class.

## Procedure
1. **Task A — Overfitting demonstration:** build a deliberately over-sized network (e.g., 2
   hidden layers, 64 units each) and train it on a small (e.g., 60-example), noisy synthetic
   dataset with a held-out 40-example validation set; plot training vs. validation loss and show
   the overfitting gap.
2. **Task B — L2 regularization:** add the L2 penalty gradient to `backward`/`update`; retrain the
   same network with $\lambda \in \{0, 0.01, 0.1\}$; plot training/validation loss for each on one
   figure.
3. **Task C — Dropout:** implement `dropout_forward`/`dropout_backward` and add a dropout layer
   (p=0.3) after the hidden layer; retrain and plot training/validation loss alongside Task A's
   no-regularization baseline.
4. **Task D — Early stopping:** implement the early-stopping loop from lecture (patience=5) on
   top of Task B's best $\lambda$; report the epoch at which it halted and the restored weights'
   validation loss vs. training the full epoch budget without early stopping.

## Expected Output
A notebook with Tasks A–D, including all required comparison plots.

## Submission
Submit `lab11.ipynb` by the end of the lab session.

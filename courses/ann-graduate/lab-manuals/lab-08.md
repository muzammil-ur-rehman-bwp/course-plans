# Lab Manual 8 — Double Descent, Weight Decay, and Dropout

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Reproduce a small double-descent curve on a controlled dataset, and compare training/validation
curves with and without weight decay and dropout.

## Setup
Create `lab08.ipynb`. Start from the lecture's `min_norm_least_squares` double-descent code.

## Procedure
1. **Task A — Double-descent curve:** reproduce the lecture's random-feature double-descent
   experiment; plot test MSE vs. width for widths spanning well below and well above
   `n_train=40`, marking the interpolation threshold on the plot.
2. **Task B — Vary training-set size:** repeat Task A with `n_train=20` and `n_train=80`; report
   how the location of the interpolation-threshold peak shifts, and explain why in one sentence.
3. **Task C — Weight decay:** train a small MLP classifier on a synthetic dataset with and
   without L2 weight decay (try at least 2 values of $\lambda$); plot train/validation loss
   curves for all runs on one chart.
4. **Task D — Dropout:** repeat Task C's comparison using dropout (try at least 2 dropout rates)
   instead of weight decay; report which regularizer (and which strength) gave the best
   validation loss on this dataset.

## Expected Output
A notebook with Tasks A–D, including the double-descent plot (with threshold marked) and both
regularization comparison plots.

## Submission
Submit `lab08.ipynb` by the end of the lab session.

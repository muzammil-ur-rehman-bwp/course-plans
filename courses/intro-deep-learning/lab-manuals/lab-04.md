# Lab Manual 4 — A Small CNN With a Residual Block on CIFAR-10

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Implement a residual block; build and train a small CNN (including the residual block) on
CIFAR-10 or Fashion-MNIST.

## Setup
Create `lab04.ipynb`. Download CIFAR-10 via `torchvision.datasets.CIFAR10(download=True)` (or use
Fashion-MNIST if CIFAR-10 download is unavailable in your environment).

## Procedure
1. **Task A — Residual block:** implement `ResidualBlock` as shown in lecture; verify that its
   output shape matches its input shape (required for the skip connection's addition to work).
2. **Task B — Plain vs. residual CNN:** build two CNNs of comparable depth — one plain stacked
   version, one using at least one `ResidualBlock` — and train both for the same number of epochs
   on CIFAR-10/Fashion-MNIST.
3. **Task C — Comparison:** plot training/validation accuracy curves for both models on one chart;
   report final test accuracy and parameter count for each.
4. **Task D — Discussion:** in a markdown cell, discuss whether the residual version trained more
   stably or reached higher accuracy, and relate the result to the degradation-problem explanation
   from lecture.

## Expected Output
A notebook with Tasks A–D, both models' curves plotted together, and the written discussion.

## Submission
Submit `lab04.ipynb` by the end of the lab session.

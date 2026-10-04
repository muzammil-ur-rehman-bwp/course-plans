# Lab Manual 13 — Neural Networks Survey: A Perceptron from Scratch

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Implement a perceptron from scratch in Python; train it on AND/OR; demonstrate its failure on
XOR.

## Setup
Create `lab13.ipynb`; use the `Perceptron` class and `train` function from the lecture content
as a starting point.

## Procedure
1. **Task A — Perceptron class:** implement/paste `Perceptron` (`predict`, `train_step`) and
   `train`; train on `AND_DATA` and report the learned weights/bias and epochs to convergence.
2. **Task B — OR gate:** train a fresh `Perceptron` on OR data; report weights/bias and epochs
   to convergence; compare the learned decision boundary (conceptually) to the AND perceptron's.
3. **Task C — XOR failure:** train a fresh `Perceptron` on `XOR_DATA` for at least 50 epochs;
   show that total error never reaches 0, and plot (or tabulate) the total error per epoch.
4. **Task D — Optional plot:** using `matplotlib`, plot the AND perceptron's decision boundary
   (the line `w1*x1 + w2*x2 + b = 0`) against the 4 training points, visually confirming
   separability.

## Expected Output
A notebook with Tasks A–D; AND and OR perceptrons must converge to zero error; the XOR
perceptron must demonstrably fail to converge.

## Submission
Submit `lab13.ipynb` by the end of the lab session.

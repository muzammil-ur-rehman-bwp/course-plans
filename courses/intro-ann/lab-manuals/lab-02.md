# Lab Manual 2 — The Perceptron and the XOR Limitation

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement the perceptron and its learning rule from scratch; train it on AND/OR; demonstrate (not
"fix") its failure on XOR.

## Setup
Create `lab02.ipynb`.

## Procedure
1. **Task A — Perceptron implementation:** implement `perceptron_train` and `perceptron_predict`
   as shown in lecture.
2. **Task B — AND/OR:** train on AND and OR separately; print predictions and confirm 100%
   training accuracy for both.
3. **Task C — Convergence plot:** track the number of misclassified training examples after each
   epoch while training on OR; plot misclassifications vs. epoch and confirm it reaches zero.
4. **Task D — XOR:** train the same code, unmodified, on the XOR dataset for at least 100 epochs;
   plot misclassifications vs. epoch and show that it never reaches zero. In a markdown cell,
   explain why, referencing linear separability.

## Expected Output
A notebook with Tasks A–D, including the two convergence plots (OR vs. XOR) side by side.

## Submission
Submit `lab02.ipynb` by the end of the lab session.

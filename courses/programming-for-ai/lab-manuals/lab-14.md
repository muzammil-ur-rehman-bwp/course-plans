# Lab Manual 14 — Neural Networks I: Forward Pass from Scratch

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Implement a perceptron and a 2-layer feedforward network's forward pass in NumPy.

## Setup
Create `lab14.ipynb`.

## Procedure
1. **Task A — Perceptron:** implement a single perceptron (`sigmoid` activation); show it can
   learn AND/OR (if time allows, via manually set weights) but demonstrate it cannot represent
   XOR.
2. **Task B — Activation functions:** implement `sigmoid`, `relu`, `softmax`; plot each.
3. **Task C — Forward pass:** implement `forward_pass(x, W1, b1, W2, b2)` for a 2-layer network
   (ReLU hidden layer, sigmoid output) as shown in lecture.
4. **Task D — Hand-check:** for a given tiny input/weight set, compute the forward pass by hand
   and verify it matches your code's output exactly.

## Expected Output
A notebook with Tasks A–D.

## Submission
Submit `lab14.ipynb` by the end of the lab session.

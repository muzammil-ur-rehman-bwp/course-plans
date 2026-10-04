# Lab Manual 9 — Weight Initialization Experiments

**Duration:** 3 hours (shortened due to midterm earlier in the week) | **Prerequisite:** Week 9 lecture

## Objectives
Compare training behavior under zero, small-random, Xavier, and He initialization.

## Setup
Create `lab09.ipynb`, reusing the Week 8 `NeuralNetwork` class.

## Procedure
1. **Task A — Zero init:** initialize both weight matrices to zero; train on the Week 8 XOR
   dataset for 2000 epochs; plot the loss curve and confirm it fails to decrease below a trivial
   level (consistent with the symmetry argument).
2. **Task B — Small random vs. Xavier vs. He:** add `xavier_init` and `he_init` as alternative
   initializers to `NeuralNetwork`; train three versions of the network (small random std=0.5,
   Xavier, He) on the same dataset and the same number of epochs; plot all three loss curves on
   one figure.
3. **Task C — Activation statistics:** build a deeper network (4 hidden layers, 10 units each,
   sigmoid activations) using each initializer; for a single forward pass on a fixed input batch,
   record and print the mean and standard deviation of each layer's activations. Identify which
   initializer keeps activation statistics most stable across depth.
4. **Task D — Reflection:** in a markdown cell, relate Task A's result to the symmetry argument
   and Task C's result to the vanishing-gradient discussion from lecture.

## Expected Output
A notebook with Tasks A–D, including the required plots and printed activation statistics table.

## Submission
Submit `lab09.ipynb` by the end of the lab session.

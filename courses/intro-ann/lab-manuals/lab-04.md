# Lab Manual 4 — A General Forward Pass and Solving XOR with an MLP

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Implement an arbitrary-depth forward pass function in NumPy and use it to reproduce the
hand-designed XOR-solving network from lecture.

## Setup
Create `lab04.ipynb`.

## Procedure
1. **Task A — General forward pass:** implement `forward_pass(x, weights, biases, activations)`
   exactly as shown in lecture, accepting lists of arbitrary length.
2. **Task B — XOR by hand-designed weights:** reproduce the lecture's XOR-solving weights; print
   the output for all four input combinations and confirm they exactly match the XOR truth table.
3. **Task C — Three-layer network:** extend the XOR network with an extra hidden layer that is an
   identity pass-through (weights = identity matrix, bias = 0) between the existing hidden and
   output layers; confirm the output is unchanged, demonstrating that `forward_pass` correctly
   handles 3+ layers.
4. **Task D — Shape assertions:** add an assertion inside `forward_pass` that checks `W.shape[1]
   == a.shape[0]` before each layer's matrix multiply, with a clear error message; demonstrate it
   firing correctly by intentionally passing a malformed weight matrix.

## Expected Output
A notebook with Tasks A–D; Task B's printed output must exactly match the XOR truth table.

## Submission
Submit `lab04.ipynb` by the end of the lab session.

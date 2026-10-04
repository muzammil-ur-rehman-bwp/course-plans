# Lab Manual 2 — Advanced CNN Building Blocks

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement a residual block, a dense block, and a depthwise separable convolution block in
PyTorch, and compare their parameter counts.

## Setup
Create `lab02.ipynb`. Start from the lecture's `ResidualBlock`, `DenseBlock`, and
`DepthwiseSeparableConv` implementations.

## Procedure
1. **Task A — Residual block:** implement `ResidualBlock`; verify output shape matches input
   shape (required for the skip addition).
2. **Task B — Dense block:** implement a 4-layer `DenseBlock` with growth rate 12 on a 32-channel
   input; print the output channel count after each layer and confirm it matches
   $32 + 12\ell$.
3. **Task C — Depthwise separable convolution:** implement `DepthwiseSeparableConv(64, 128)` and
   compare its parameter count against a standard `Conv2d(64, 128, kernel_size=3)`; confirm the
   ratio matches the formula $1/C_{out}+1/k^2$ within rounding.
4. **Task D — Assemble and train:** build a small CNN using one of each block (residual, dense,
   depthwise-separable) in sequence, and train it on CIFAR-10/Fashion-MNIST for 5 epochs,
   reporting test accuracy.

## Expected Output
A notebook with Tasks A–D; Task C's parameter-count comparison printed explicitly.

## Submission
Submit `lab02.ipynb` by the end of the lab session.

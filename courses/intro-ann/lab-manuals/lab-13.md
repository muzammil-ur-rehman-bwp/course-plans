# Lab Manual 13 — Convolution, Pooling, and a Small CNN

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Implement convolution and max-pooling by hand in NumPy; build and train a small CNN on MNIST/
Fashion-MNIST with the framework.

## Setup
Create `lab13.ipynb`.

## Procedure
1. **Task A — Convolution from scratch:** implement `conv2d(image, kernel, stride)` as shown in
   lecture; test it on a provided 6×6 test image with two different 3×3 kernels (an edge detector
   and a blur/averaging kernel) and visualize both outputs.
2. **Task B — Pooling from scratch:** implement `max_pool2d`; apply it to Task A's convolution
   outputs and visualize the downsampled results.
3. **Task C — Small CNN:** build `SmallCNN` (or your own minimal architecture) with the
   framework; train it on MNIST/Fashion-MNIST for at least 5 epochs; plot training/validation
   loss and accuracy curves.
4. **Task D — CNN vs. MLP:** compare Task C's CNN test accuracy and parameter count against
   Week 12's MLP (same dataset, similar training budget); report both numbers in a small table
   and comment on the trade-off in a markdown cell.

## Expected Output
A notebook with Tasks A–D, including all required visualizations and the comparison table.

## Submission
Submit `lab13.ipynb` by the end of the lab session.

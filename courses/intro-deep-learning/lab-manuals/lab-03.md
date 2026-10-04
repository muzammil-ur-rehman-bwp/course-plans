# Lab Manual 3 — Convolution Arithmetic, Receptive Field, and Pooling

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Compute convolution output sizes and receptive fields by hand; implement and verify multi-channel
convolution and pooling with `nn.Conv2d`/`nn.MaxPool2d`.

## Setup
Create `lab03.ipynb`.

## Procedure
1. **Task A — Output-size calculations:** for five provided (input size, kernel size, stride,
   padding) configurations, compute the output size by hand using the formula from lecture, then
   verify each with `nn.Conv2d` by checking the resulting tensor's shape.
2. **Task B — Receptive field:** for a stack of 3, 5, and 7 convolutional layers (kernel 3,
   stride 1), compute the receptive field by hand using the formula from lecture, then confirm by
   constructing each stack and reasoning about which input region could affect one output unit.
3. **Task C — Multi-channel convolution and parameter count:** build a `nn.Conv2d(3, 16,
   kernel_size=3, padding=1)` layer; compute its parameter count by hand and verify against
   `sum(p.numel() for p in layer.parameters())`.
4. **Task D — Pooling:** apply `nn.MaxPool2d(2, 2)` to a feature map from Task C; verify the
   output shape matches the expected halved spatial dimensions.

## Expected Output
A notebook with Tasks A–D, all hand calculations shown in markdown cells next to their code
verification.

## Submission
Submit `lab03.ipynb` by the end of the lab session.

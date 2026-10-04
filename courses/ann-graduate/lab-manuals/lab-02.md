# Lab Manual 2 — Universal Approximation: Width, Approximation Error, and Depth

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Empirically demonstrate the Universal Approximation Theorem's content by fitting a
single-hidden-layer network to a target function as width grows, and compare a wide-shallow
network to a narrow-deep network of similar parameter count.

## Setup
Create `lab02.ipynb`. Reuse the lecture's `fit_shallow_network` function or write your own.

## Procedure
1. **Task A — Width sweep:** fit a shallow (1 hidden layer) network to
   $f(x)=\sin(6x)+0.5\sin(17x)$ on $[0,1]$ for widths $\{2,4,8,16,32,64,128\}$; plot training MSE
   vs. width on a log-y axis.
2. **Task B — Visual fit quality:** for widths $2$, $16$, and $128$, plot the fitted function
   against the true target on the same axes; discuss what "more units" buys visually.
3. **Task C — Wide-shallow vs. narrow-deep:** build a narrow-deep network (e.g., 4 hidden layers
   of width 8) with roughly the same total parameter count as a wide-shallow network (1 hidden
   layer of width ~32–40); train both on a harder target with multiple length scales (e.g.,
   $f(x) = \text{sign}(\sin(2^4 \pi x))$, a square wave) and compare final MSE.
4. **Task D — Discussion:** write 3–4 sentences connecting Task C's result to the depth-vs-width
   expressivity argument from lecture.

## Expected Output
A notebook with Tasks A–D, including the two required plots and the written discussion.

## Submission
Submit `lab02.ipynb` by the end of the lab session.

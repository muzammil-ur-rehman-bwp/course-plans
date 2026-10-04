# Lab Manual 11 — Gradient Flow and a Toy Attention Visualization

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Compare gradient-flow magnitude through a deep plain feedforward network versus an equivalent
network with skip connections, both near identity initialization, and build a toy
attention-weight visualization illustrating the expressivity argument.

## Setup
Create `lab11.ipynb`. Start from the lecture's `make_plain_net`/`make_residual_net` functions.

## Procedure
1. **Task A — Depth sweep:** compute the input-gradient norm (as in the lecture) for both plain
   and residual networks at depths $\{5, 10, 20, 40, 80\}$; plot both curves (log-y axis) on one
   chart.
2. **Task B — Non-zero residual init:** repeat Task A's depth-80 case, but initialize the
   residual branch's last layer with small-random (not exactly zero) weights at 3 different
   scales; report how far from "near-identity" initialization the gradient-flow advantage
   persists.
3. **Task C — Toy attention:** implement a minimal single-head self-attention layer (no
   positional encoding needed) over a short toy sequence (length 10, dimension 4); visualize the
   resulting attention-weight matrix as a heatmap for 2 different random initializations.
4. **Task D — Path-length argument, concretely:** for a 1D convolution with kernel size 3, compute
   by hand (or by code) how many layers are needed for position $0$ to influence position $20$,
   and compare this to attention's path length for the same pair of positions.

## Expected Output
A notebook with Tasks A–D, including the Task A plot and the Task C heatmaps.

## Submission
Submit `lab11.ipynb` by the end of the lab session.

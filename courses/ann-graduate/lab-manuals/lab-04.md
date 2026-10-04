# Lab Manual 4 — Initialization Variance Experiment

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Measure layer-wise activation variance across network depth under zero, naive-random, Xavier, and
He initialization, and connect the empirical curves to the lecture's derived formulas.

## Setup
Create `lab04.ipynb`. Reuse the lecture's `run_depth_experiment` function or write your own.

## Procedure
1. **Task A — Four schemes, tanh:** run the depth experiment (depth 30, width 256) for `zero`,
   `naive`, and `xavier` initialization with tanh activation; plot activation variance vs. layer
   index (log-y axis) for all three on one chart.
2. **Task B — Four schemes, ReLU:** repeat for `naive` and `he` initialization with ReLU
   activation; plot on a second chart.
3. **Task C — Predicted vs. observed:** for Xavier (tanh) and He (ReLU), compute the
   lecture-derived predicted steady-state variance ratio per layer (should be $\approx 1$) and
   compare it numerically to the observed ratio between layer 1 and layer 30's variance.
4. **Task D — Gradient-side check:** instead of the forward activation variance, measure the
   variance of $\partial L/\partial a^{(l)}$ (via backpropagating a random scalar loss) across
   depth under Xavier vs. naive initialization, and report whether gradient variance is also
   preserved under Xavier.

## Expected Output
A notebook with Tasks A–D, including both required plots and the Task C/D numerical comparisons.

## Submission
Submit `lab04.ipynb` by the end of the lab session.

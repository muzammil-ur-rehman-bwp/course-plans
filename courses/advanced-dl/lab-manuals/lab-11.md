# Lab Manual 11 — Compute-Optimal Allocation and Checkpoint Intervals

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Compute compute-optimal $(N,D)$ allocations against a naive baseline, and derive/compute an
optimal checkpoint interval for a toy training-run specification.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab11.ipynb`.

## Procedure
1. **Task A — Allocation functions:** implement `compute_optimal_allocation` and
   `naive_allocation` exactly as in the Week 11 lecture content, with illustrative exponents
   $a=0.46, b=0.54$.
2. **Task B — Loss comparison:** using a provided toy fitted loss function $L(N,D)$, compute and
   plot the loss each allocation strategy would achieve across a range of compute budgets $C$,
   showing the naive allocation's growing gap from compute-optimal as $C$ increases.
3. **Task C — Checkpoint interval:** implement `optimal_checkpoint_interval` and
   `expected_lost_compute` exactly as in lecture; for a toy specification (checkpoint cost,
   assumed failure rate), compute $\tau^*$ and plot `expected_lost_compute` vs. $\tau$ to confirm
   the computed value sits at the curve's minimum.
4. **Task D — Scoping statement:** write a precise, 100–150 word statement distinguishing this
   week's compute-allocation question from the sibling *Advanced Artificial Neural Network*
   course's theoretical "why do power laws hold" question.

## Expected Output
A notebook with four clearly labeled sections (A–D) and the two required plots.

## Submission
Submit `lab11.ipynb` via the course submission system by the end of the lab session.

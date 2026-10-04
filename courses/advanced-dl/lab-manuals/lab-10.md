# Lab Manual 10 — Evolutionary and Differentiable Neural Architecture Search

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement a small evolutionary search over a toy architecture space and a minimal differentiable-
NAS-style operation relaxation, and compare their search cost against full-training evaluation.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab10.ipynb`.

## Procedure
1. **Task A — Search space and fitness:** define a toy search space (e.g., per-layer width
   $\in\{16,32,64\}$, activation $\in\{\mathrm{ReLU},\mathrm{GELU}\}$ for a 3-layer MLP) and a
   cheap fitness function (validation accuracy after a fixed, small number of training steps).
2. **Task B — Evolutionary search:** implement `mutate` and `evolutionary_search` exactly as in
   the Week 10 lecture content; run it and plot best-found fitness across generations.
3. **Task C — Differentiable NAS:** implement `MixedOp` exactly as in lecture, inserted at one
   position in a 3-layer MLP; train it jointly with the rest of the network's weights on the same
   toy task and plot $\mathrm{softmax}(\alpha)$'s evolution over training.
4. **Task D — Cost comparison:** estimate the total training steps spent by Task B's evolutionary
   search and Task C's differentiable search, and compare both to the (much larger) hypothetical
   cost of fully training every candidate in the Task A search space from scratch to convergence.

## Expected Output
A notebook with four clearly labeled sections (A–D), the fitness-vs-generation plot, and the
$\alpha$-evolution plot.

## Submission
Submit `lab10.ipynb` via the course submission system by the end of the lab session.

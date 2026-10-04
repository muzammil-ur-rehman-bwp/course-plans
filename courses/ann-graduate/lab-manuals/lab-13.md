# Lab Manual 13 — Iterative Magnitude Pruning and the Lottery Ticket Control

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Implement iterative magnitude pruning to find a sparse mask, compare a winning-ticket retraining
run against a random-reinitialization control at the same sparsity, and run a minimal
knowledge-distillation experiment.

## Setup
Create `lab13.ipynb`. Start from the lecture's pruning/training code.

## Procedure
1. **Task A — Sparsity sweep:** repeat the lecture's pruning procedure at sparsities
   $\{30\%, 50\%, 70\%, 90\%\}$; for each, report dense/winning-ticket/random-control test
   accuracy in a table.
2. **Task B — Iterative vs. one-shot pruning:** compare one-shot pruning (prune directly to 70%
   in a single step) against the lecture's iterative procedure (e.g., 3 rounds of ~37% each,
   compounding to ~70%); report which gives better winning-ticket accuracy at the same final
   sparsity.
3. **Task C — Distillation:** train a large "teacher" network to convergence; train a small
   "student" network (a) from scratch on hard labels only, and (b) via distillation against the
   teacher's softened output probabilities (use a temperature-scaled softmax); compare student
   test accuracy in both conditions.
4. **Task D — Discussion:** state, in 2–3 sentences, what Task A's accuracy gap (winning ticket
   vs. random control) at the *highest* sparsity tested suggests about how much of the effect is
   initialization-dependent versus architecture-dependent.

## Expected Output
A notebook with Tasks A–D, including Task A's table and Task C's accuracy comparison.

## Submission
Submit `lab13.ipynb` by the end of the lab session.

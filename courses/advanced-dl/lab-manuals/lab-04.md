# Lab Manual 4 — Mixture-of-Experts Layer

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Implement a top-$k$ MoE layer and a load-balancing auxiliary loss from scratch, and compare
FLOPs/parameters/accuracy against matched dense baselines.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab04.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `Expert` and `TopKMoE` exactly as in the Week 4 lecture
   content, with $N=8$ experts, $k=2$, on a toy sequence-classification task (e.g., classifying
   short synthetic token sequences into categories).
2. **Task B — FLOP/parameter accounting:** compute and report, analytically, the MoE layer's
   per-token FLOP cost and total parameter count, and compare both to (i) a dense FFN with the
   same per-token FLOPs as 2 experts, and (ii) a dense FFN with the same total parameters as all
   8 experts combined.
3. **Task C — Load balancing:** implement `load_balancing_loss` exactly as in lecture; train the
   MoE model both with and without the auxiliary loss added (small coefficient, e.g. 0.01), and
   plot the resulting per-expert token-usage histogram in each condition.
4. **Task D — Accuracy comparison:** train and report final validation accuracy for the MoE model
   (with load balancing) and both dense baselines from Task B at their respective matched
   FLOP/parameter budgets.

## Expected Output
A notebook with four clearly labeled sections (A–D), the FLOP/parameter table, and the two
usage-histogram plots.

## Submission
Submit `lab04.ipynb` via the course submission system by the end of the lab session.

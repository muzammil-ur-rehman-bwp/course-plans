# Lab Manual 2 — Empirical Fano-Bound Sanity Check

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement the two-point testing simulation from the Week 2 lecture content and empirically
confirm the $\Theta(1/\sqrt n)$ critical separation scale the Fano-inequality-based minimax lower
bound predicts for the Gaussian location family.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab02.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `two_point_test_error(n, eps, n_trials, rng)` exactly as
   in the Week 2 lecture content.
2. **Task B — Critical-scale sweep:** for $n\in\{20,50,100,200,500,1000\}$, compute
   $\epsilon^\star=\sqrt{\ln2/(4n)}$ and the empirical testing error at $\epsilon^\star$ and at
   $3\epsilon^\star$; tabulate both.
3. **Task C — Scaling plot:** plot $\epsilon^\star$ against $n$ on a log-log axis and confirm the
   slope is consistent with $\epsilon^\star \propto n^{-1/2}$.
4. **Task D — Mini-challenge:** repeat the 4-point packing-set derivation from the Week 2
   in-class exercise numerically: implement a 4-point testing simulation (nearest-neighbor
   decision rule) and confirm its critical separation scale is also $\Theta(1/\sqrt n)$, with a
   different (larger) constant than the 2-point case.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required log-log plot.

## Submission
Export/submit `lab02.ipynb` via the course submission system by the end of the lab session.

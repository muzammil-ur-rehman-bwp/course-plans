# Lab Manual 3 — Mean-Field Signal Propagation vs. Xavier/He

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Simulate variance propagation through depth using the mean-field recursion, and confirm it
recovers the Xavier/Glorot and He fixed points, plus the mis-scaled-ReLU failure mode.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab03.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `propagate_variance` exactly as in the Week 3 lecture
   content, and reproduce the three traces shown there (linear/Xavier, ReLU/He, ReLU/mis-scaled).
2. **Task B — Fixed-point verification:** for the linear and He-scaled ReLU cases, verify
   numerically that $q$ stays within 5% of its starting value `q0` across all 40 layers; for the
   mis-scaled case, report the layer at which $q$ has dropped below 1% of `q0`.
3. **Task C — Leaky-ReLU extension:** implement `propagate_variance` for leaky-ReLU with at least
   3 values of the negative slope $\alpha \in (0,1)$, and empirically find the $\sigma_w^2$ that
   keeps $q$ fixed for each, comparing against your Week 3 in-class-exercise derivation.
4. **Task D — Correlation propagation (mini-challenge):** implement the companion correlation
   recursion for two inputs with initial correlation $c^0 = 0.9$ under the ReLU/He-scaled setting,
   and plot $c^l$ vs. depth, reporting whether it appears to approach a fixed point below 1.

## Expected Output
A notebook with four clearly labeled sections (A–D), including all required plots and reported
numerical comparisons.

## Submission
Export/submit `lab03.ipynb` via the course submission system by the end of the lab session.

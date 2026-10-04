# Lab Manual 6 — Sharpness Estimation and SAM

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Implement a Hessian-top-eigenvalue estimator via power iteration and compare SAM-trained vs.
SGD-trained sharpness and test accuracy on the same task.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab06.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `hvp`, `top_hessian_eigenvalue`, and `sam_step` exactly
   as in the Week 6 lecture content.
2. **Task B — Baseline comparison:** train two copies of the same architecture on the same data —
   one with plain SGD, one with `sam_step` — for the same number of steps and the same base
   learning rate, and report final training loss, test loss, and `top_hessian_eigenvalue` for
   each.
3. **Task C — Rho sensitivity:** repeat the SAM run for at least 3 values of `rho`, reporting
   sharpness and test loss for each, and discuss the tradeoff observed.
4. **Task D — Reparameterization check (mini-challenge):** for the plain-SGD-trained model, apply
   a function-preserving rescaling (as in Week 6 §3: multiply one linear layer's weights by
   $\alpha$ and the following layer's by $1/\alpha$, for a ReLU network) for at least 2 values of
   $\alpha \ne 1$, and report `top_hessian_eigenvalue` before and after rescaling, confirming the
   function computed is unchanged (same test loss) while sharpness changes.

## Expected Output
A notebook with four clearly labeled sections (A–D), including all required comparisons.

## Submission
Export/submit `lab06.ipynb` via the course submission system by the end of the lab session.

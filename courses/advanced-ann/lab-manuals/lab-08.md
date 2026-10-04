# Lab Manual 8 — Fitting Scaling-Law Curves

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Fit a power-law curve to loss-vs-model-size data and read off the fitted scaling exponent,
and examine how fit quality and exponent estimates respond to noise and sample count.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab08.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `fit_power_law` exactly as in the Week 8 lecture
   content, and reproduce its synthetic-data fit.
2. **Task B — Noise sensitivity:** repeat the fit for at least 4 noise levels (including the
   lecture content's `0.01` and at least one substantially larger value), reporting the fitted
   `alpha` and `N_c` at each, and discuss how noise affects exponent-estimate reliability.
3. **Task C — Sparse-data sensitivity:** repeat the fit using only 3 of the 7 size points (choose
   a reasonable, justified subset), and compare the resulting fitted exponent to the full-data fit
   from Task A.
4. **Task D — Mini-challenge:** generate synthetic data from a joint model-size-and-data-size
   power law, $L(N,D) = (N_c/N)^{\alpha_N} + (D_c/D)^{\alpha_D}$, for a range of $(N,D)$ pairs, and
   fit $\alpha_N$ and $\alpha_D$ separately by holding one variable fixed while sweeping the other
   (a simplified echo of how compute-optimal-scaling-style analyses separate the two effects).

## Expected Output
A notebook with four clearly labeled sections (A–D), including all required fits and discussion.

## Submission
Export/submit `lab08.ipynb` via the course submission system by the end of the lab session.

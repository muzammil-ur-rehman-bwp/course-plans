# Lab Manual 7 — Chinese Restaurant Process Sampler and Infinite Mixture Model

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Implement a CRP sampler, verify seating probabilities and cluster-count growth, and build a
CRP-based infinite Gaussian mixture generator.

## Setup
1. Reuse your virtual environment.
2. Create `lab07.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `crp_sample(n, alpha, rng)` and
   `crp_gaussian_mixture(n, alpha, H_scale, obs_noise, rng)` exactly as in the Week 7 lecture
   content.
2. **Task B — Growth verification:** for $\alpha\in\{0.5,2,5\}$ and $n=2000$, run the CRP and
   compare the observed number of occupied tables $K_n$ to the predicted $\alpha\ln n$, as in the
   lecture content.
3. **Task C — Seating-probability check:** run 2000 independent CRP simulations to $n=10$ with
   $\alpha=2$, and empirically estimate $\mathbb{P}(\text{customer 11 starts a new table})$;
   compare to the exact value $\alpha/(n+\alpha)=2/12$.
4. **Task D — Mini-challenge:** build the infinite Gaussian mixture generator for $\alpha=3$,
   $H\sim\mathcal N(0,25)$, generate $n=1000$ observations, and plot a histogram of the generated
   data overlaid with vertical lines at each table's true $\theta_k$ location, confirming the
   data visibly clusters around the table parameters.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required plots.

## Submission
Export/submit `lab07.ipynb` via the course submission system by the end of the lab session.

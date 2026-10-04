# Lab Manual 6 — Truncated Stick-Breaking Dirichlet Process Sampler

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Implement a truncated stick-breaking sampler, verify the weight-sum-to-1 property empirically,
and explore the effect of the concentration parameter $\alpha$ on cluster concentration.

## Setup
1. Reuse your virtual environment.
2. Create `lab06.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `stick_breaking(alpha, K, base_sampler, rng)` exactly
   as in the Week 6 lecture content.
2. **Task B — Weight-sum verification:** for $\alpha\in\{0.5,2,10\}$ and $K=1000$, confirm the
   sum of weights is within $10^{-4}$ of 1, and report the truncation's residual tail mass
   $1-\sum_k \pi_k$ for a smaller $K=50$ to show why larger truncations are needed for larger
   $\alpha$.
3. **Task C — Sample histogram:** draw $n=500$ samples from $G\sim\mathrm{DP}(\alpha,H)$ for
   $\alpha\in\{0.5,2,10\}$ with $H=\mathcal N(0,9)$, and plot histograms for each, confirming the
   qualitative pattern described in lecture (few sharp spikes at small $\alpha$; smoother, more
   $H$-like shape at large $\alpha$).
4. **Task D — Mini-challenge:** empirically estimate, for each $\alpha$, the number of distinct
   atoms carrying at least 90% of the drawn samples' total weight, and relate the trend across
   $\alpha$ to the cluster-concentration discussion in lecture.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required histograms.

## Submission
Export/submit `lab06.ipynb` via the course submission system by the end of the lab session.

# Lab Manual 4 — Simulating the Marchenko–Pastur Law

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Simulate the empirical spectral distribution of a sample covariance matrix across several aspect
ratios $\gamma=p/n$ and confirm convergence to the Marchenko–Pastur density.

## Setup
1. Reuse your virtual environment.
2. Create `lab04.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `mp_density(x, gamma)` and the simulation loop exactly
   as in the Week 4 lecture content, for $\gamma\in\{0.2,0.5,0.9\}$.
2. **Task B — Edge verification:** for each $\gamma$, compute the predicted support edges
   $a=(1-\sqrt\gamma)^2$, $b=(1+\sqrt\gamma)^2$ and report the fraction of simulated eigenvalues
   falling outside $[a,b]$ (should be small/zero, up to finite-size fluctuation).
3. **Task C — Largest-eigenvalue tracking:** for $p=250,n=500$ ($\gamma=0.5$), run 30 independent
   simulated $\hat\Sigma$'s (true covariance $=I$) and report the mean and standard deviation of
   the largest sample eigenvalue, comparing the mean against the predicted edge $b$.
4. **Task D — Mini-challenge:** repeat Task C for $\gamma=0.95$ and $\gamma=0.99$ and report how
   the smallest sample eigenvalue behaves as $\gamma\to1$, relating your observation to the
   lecture content's near-singularity discussion.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output, including the overlay plot from the lecture content's §5 code.

## Submission
Export/submit `lab04.ipynb` via the course submission system by the end of the lab session.

# Lab Manual 2 — Toy SDE Score-Based Diffusion

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement a toy 2-D score network trained by denoising score matching, and an Euler-Maruyama
reverse-SDE sampler, verifying the DDPM-loss correspondence from lecture.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab02.ipynb`.

## Procedure
1. **Task A — Data:** generate 2,000 samples from a 2-D two-component Gaussian mixture (means at
   least 4 units apart, unit-scale covariance).
2. **Task B — Score network and training:** implement `ScoreNet` and `score_matching_loss`
   exactly as in the Week 2 lecture content; train for at least 2,000 steps and plot the training
   loss curve.
3. **Task C — Sampling:** implement `reverse_sde_sample` exactly as in lecture; generate 1,000
   samples and overlay them on the true mixture's density contours (a 2-D scatter plot with
   contour lines is sufficient).
4. **Task D — Discretization-error sweep:** run the sampler with `n_steps` ∈ {10, 50, 200, 1000}
   and visually/quantitatively (e.g., via a 2-Wasserstein-distance-style or simple nearest-
   component-mean comparison) compare sample fidelity, confirming more steps improve fidelity and
   very few steps visibly bias samples.

## Expected Output
A notebook with four clearly labeled sections (A–D), the training curve, the overlay plot, and
the `n_steps` comparison plot.

## Submission
Submit `lab02.ipynb` via the course submission system by the end of the lab session.

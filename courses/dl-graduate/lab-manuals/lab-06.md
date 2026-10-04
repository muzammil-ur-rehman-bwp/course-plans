# Lab Manual 6 — A 1-D Normalizing Flow and the Diffusion Forward Process

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Fit a 1-D normalizing flow to synthetic data by maximum likelihood; implement and visualize the
diffusion forward noising process.

## Setup
Create `lab06.ipynb`. Start from the lecture's `AffineFlow` and `forward_diffusion_sample`.

## Procedure
1. **Task A — Affine flow fit:** fit `AffineFlow` by maximum likelihood to 1-D data sampled from
   $\mathcal N(3, 4)$; confirm the learned mean/std converge close to the true values.
2. **Task B — Flow limitation:** fit the same affine flow to **bimodal** 1-D data (e.g., a mixture
   of two well-separated Gaussians); observe and explain in a markdown cell why a single affine
   flow cannot capture bimodality (it can only represent a single Gaussian shape).
3. **Task C — Diffusion forward process:** implement `make_beta_schedule` and
   `forward_diffusion_sample`; starting from a 2-D toy "image" (or a 1-D signal), visualize
   (plot) the signal at $t\in\{0, T/4, T/2, 3T/4, T-1\}$ and confirm it visibly approaches noise.
4. **Task D — Closed-form check:** empirically verify, by sampling many times at a fixed $t$,
   that the empirical mean/variance of $x_t$ matches the closed-form
   $\sqrt{\bar\alpha_t}x_0$/$(1-\bar\alpha_t)$ predicted by the forward-process formula.

## Expected Output
A notebook with Tasks A–D; Task C's visualization and Task D's empirical-vs-closed-form
comparison printed.

## Submission
Submit `lab06.ipynb` by the end of the lab session.

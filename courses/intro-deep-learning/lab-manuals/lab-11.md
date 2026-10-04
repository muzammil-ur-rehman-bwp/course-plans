# Lab Manual 11 — Variational Autoencoders

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Implement the reparameterization trick and the ELBO loss; build and train a simple VAE on
MNIST/Fashion-MNIST; sample new images from the trained decoder.

## Setup
Create `lab11.ipynb`.

## Procedure
1. **Task A — Reparameterization trick:** implement `reparameterize(mu, logvar)` as in lecture;
   verify, for repeated calls with the same `mu`/`logvar`, that returned samples vary (confirming
   randomness is present) while gradients still flow through `mu`/`logvar` (confirm via a dummy
   `.backward()` call).
2. **Task B — ELBO loss:** implement `vae_loss(x_hat, x, mu, logvar)` as in lecture; verify the
   KL term is zero when `mu=0` and `logvar=0` (matching the prior exactly).
3. **Task C — Train the VAE:** build and train the `VAE` model from lecture on
   MNIST/Fashion-MNIST; plot the total loss, reconstruction term, and KL term separately across
   training.
4. **Task D — Sampling and interpolation:** sample at least 16 new images from
   `z ~ N(0, I)` via the trained decoder; additionally, interpolate linearly between two test
   images' latent codes and decode several interpolated points, visualizing the transition.

## Expected Output
A notebook with Tasks A–D, the three-term loss plot, and the sampling/interpolation
visualizations.

## Submission
Submit `lab11.ipynb` by the end of the lab session.

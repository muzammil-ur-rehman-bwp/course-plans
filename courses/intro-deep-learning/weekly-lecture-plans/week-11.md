# Week 11 Lecture Plan — Introduction to Deep Learning
## Topic: Generative Models I — Variational Autoencoders

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the generative modeling problem as distinct from reconstruction. (*Understand*)
2. Apply the reparameterization trick to enable differentiable sampling. (*Apply*)
3. Apply the ELBO loss (reconstruction + KL term) to train a VAE in PyTorch. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | From autoencoder to generative model | Why a plain autoencoder's latent space is not well-suited to sampling new data |
| 0:15–0:40 | The probabilistic encoder | Encoding to a distribution (mean, log-variance) instead of a point |
| 0:40–1:00 | The reparameterization trick | Why direct sampling blocks gradients; `z = mu + sigma * epsilon` |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | The ELBO loss | Reconstruction term + closed-form Gaussian KL-divergence term, derived and explained |
| 1:40–2:00 | Live demo | Simple VAE on MNIST/Fashion-MNIST, trained and sampled from |

### Materials/Equipment
- Live-coding environment, PyTorch
- Handout: ELBO derivation and the closed-form Gaussian KL term

### Formative Check (in-class)
Explain why the reparameterization trick is necessary for backpropagating through a sampling
step, and what would go wrong if `z` were sampled directly from `N(mu, sigma^2)` without it.

### Link to Lab/Assessment
Lab 11: Build and train a simple VAE on MNIST/Fashion-MNIST; sample new images from the trained
decoder and inspect the latent space.

### Assessment Note
**Capstone project proposal is due** this week.

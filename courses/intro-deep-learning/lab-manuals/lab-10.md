# Lab Manual 10 — Autoencoders and Denoising Autoencoders

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Build and train a (denoising) autoencoder on MNIST/Fashion-MNIST; visualize reconstructions and
the latent space; connect a linear autoencoder to PCA.

## Setup
Create `lab10.ipynb`.

## Procedure
1. **Task A — Plain autoencoder:** implement `Autoencoder` as in lecture; train on
   MNIST/Fashion-MNIST; visualize original vs. reconstructed images for several test examples.
2. **Task B — Denoising autoencoder:** add Gaussian noise to inputs (`add_noise`); train to
   reconstruct the clean target; visualize noisy input, clean target, and reconstruction
   side by side.
3. **Task C — Latent space visualization:** extract the latent code `z` for a batch of test
   images; if `latent_dim` > 2, reduce to 2D with PCA (from `sklearn` or a provided helper) for
   visualization; scatter-plot colored by class label.
4. **Task D — Linear autoencoder vs. PCA:** train the linear autoencoder from lecture (no
   activations); compare its reconstruction error against `sklearn.decomposition.PCA` with the
   same number of components, and discuss the result in a markdown cell.

## Expected Output
A notebook with Tasks A–D, all visualizations, and the Task D comparison/discussion.

## Submission
Submit `lab10.ipynb` by the end of the lab session.

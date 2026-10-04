# Lab Manual 7 — Training and Sampling a Toy Diffusion Model

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Implement the simplified DDPM-style training loss and the sampling loop, and train a small noise
predictor on 2-D synthetic data.

## Setup
Create `lab07.ipynb`. Start from the lecture's `diffusion_training_loss`, `TinyNoisePredictor`,
and `sample`.

## Procedure
1. **Task A — Training loop:** train `TinyNoisePredictor` on 2-D synthetic data (e.g., points
   sampled from a ring/annulus shape) using `diffusion_training_loss`; plot the training loss
   curve over several hundred steps.
2. **Task B — Sampling:** use the trained model's `sample` function to generate new 2-D points
   from pure noise; plot the generated points against the true data distribution.
3. **Task C — Partial training comparison:** repeat Task B using a checkpoint saved after only
   ~10% of training; compare the generated-sample quality against the fully-trained model's
   samples.
4. **Task D — Discussion:** in a markdown cell, discuss how many sequential network evaluations
   sampling required, and contrast this with how many evaluations a VAE or GAN would need to
   generate the same number of samples.

## Expected Output
A notebook with Tasks A–D; Task B's generated-vs-true scatter plot and Task C's comparison
plotted.

## Submission
Submit `lab07.ipynb` by the end of the lab session.

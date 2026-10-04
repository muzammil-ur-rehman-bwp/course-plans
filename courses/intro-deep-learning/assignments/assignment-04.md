# Assignment 4 — Generative Models and Practical Training (Weeks 10–14)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 14 | **Due:** Start of Week 16

## Instructions
Submit a single Jupyter notebook `assignment04.ipynb` answering all questions below. Show your
work (code + brief explanation) for each question.

## Questions
1. **(Autoencoder and PCA, 15 pts)** Train a linear autoencoder and compare its reconstruction
   error against `sklearn.decomposition.PCA` with the same number of components on a provided
   dataset `assignment04_data`; discuss the result in light of the PCA connection from lecture.
2. **(VAE, 25 pts)** Implement the reparameterization trick and ELBO loss; train a VAE on a
   provided image dataset; sample at least 16 new images from the trained decoder and report the
   reconstruction and KL loss terms separately across training.
3. **(GAN, 25 pts)** Implement and train a simple GAN on the same (or a provided) image dataset;
   report generator/discriminator loss curves and a grid of generated samples at the final epoch;
   state whether you observed mode collapse or instability, with evidence.
4. **(AdamW vs. Adam+L2, 15 pts)** Train the same model with `Adam(weight_decay=...)` and
   `AdamW(weight_decay=...)` using the same numeric value; report and explain any difference in
   final validation performance, referencing the per-parameter adaptive scaling argument from
   lecture.
5. **(Reflection, 20 pts)** In 5–7 sentences, compare the VAE and GAN approaches to generative
   modeling covered this semester: what does each optimize, and what trade-off (e.g., sample
   quality vs. training stability, or explicit likelihood vs. none) does each make?

## Submission
Upload `assignment04.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

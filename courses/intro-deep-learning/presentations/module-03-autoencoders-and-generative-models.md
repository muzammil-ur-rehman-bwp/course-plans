# Presentation: Module 3 — Autoencoders & Generative Models (Weeks 10–12)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 3: Autoencoders & Generative Models
2. **The autoencoder** — encoder-decoder for reconstruction, the bottleneck layer
3. **Denoising autoencoders** — reconstructing clean targets from corrupted inputs
4. **Linear autoencoders and PCA** — the subspace connection
5. **From reconstruction to generation** — why a plain autoencoder's latent space is not enough
6. **The probabilistic encoder** — outputting a distribution ($\mu$, $\log\sigma^2$) instead of a point
7. **The reparameterization trick** — `z = mu + sigma * eps`, and why it is needed
8. **The ELBO loss** — reconstruction term + closed-form Gaussian KL term
9. **Sampling from a trained VAE** — decoding from the prior $N(0, I)$
10. **The GAN minimax game** — generator vs. discriminator
11. **The adversarial training loop** — alternating updates, the role of `.detach()`
12. **GAN failure modes** — mode collapse and training instability, diagnosed from samples

**Speaker notes:** slide 7 (reparameterization trick) is the single hardest idea in this module —
walk through the "what would go wrong without it" argument explicitly rather than only presenting
the formula.

# Presentation: Module 2 — Self-Supervised & Generative Models (Weeks 5–7)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 2: Self-Supervised & Generative Models
2. **Recap** — transfer learning from the undergraduate course; today, learning without labels
   at all
3. **The pretext-task idea** — deriving training signal automatically from unlabeled data
4. **Contrastive learning & InfoNCE, derived** — the $(K{+}1)$-way classification framing
5. **SimCLR-style framing** — two augmented views, shared encoder, projection head
6. **Linear probing** — freezing the encoder; isolating representation quality
7. **Normalizing flows** — the change-of-variables formula; the 1-D affine-flow worked example
8. **The diffusion forward process** — a fixed Gaussian noising Markov chain; the closed-form
   marginal
9. **The diffusion reverse process** — the learned denoising chain; the noise-prediction
   parameterization
10. **The simplified training objective** — the DDPM-style loss; why it's tractable
11. **Diffusion sampling** — the iterative reverse loop
12. **Diffusion vs. GAN vs. VAE** — training stability, sample quality, sampling cost trade-offs

**Speaker notes:** slide 4 (InfoNCE derived) and slide 10 (the simplified diffusion objective) are
this module's two conceptual anchors — both reduce a seemingly complex objective to a simple,
familiar loss (cross-entropy; MSE regression), and that reduction is the key "aha" this module
should land for every student.

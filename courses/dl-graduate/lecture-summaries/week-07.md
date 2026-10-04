# Week 7 Summary — The Diffusion Reverse Process and Training

**Key takeaways:**
- The reverse process is a learned Gaussian chain parameterized via a noise-prediction network
  $\epsilon_\theta(x_t,t)$.
- The simplified DDPM-style training loss $\mathbb{E}\|\epsilon-\epsilon_\theta(x_t,t)\|^2$ is a
  tractable plain regression, made possible by the forward process's closed form.
- Sampling iteratively applies the learned reverse step from pure noise down to a generated
  sample — $T$ sequential network evaluations, unlike a GAN's or VAE's single pass.
- Diffusion models trade sampling-time cost for training stability and sample diversity relative
  to GANs, and for sharpness relative to a plain VAE.

**You should now be able to:** implement the simplified diffusion training loss and the sampling
loop, and contrast diffusion, GAN, and VAE training/sampling trade-offs.

**Next week:** graph neural networks I — message passing and the GCN layer; midterm review.

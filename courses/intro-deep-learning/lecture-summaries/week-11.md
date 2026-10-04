# Week 11 Summary — Generative Models I: Variational Autoencoders

**Key takeaways:**
- Generative modeling asks for a model that can sample new, realistic data, which a plain
  autoencoder's latent space does not reliably support.
- A VAE's encoder outputs a distribution (mean and log-variance) rather than a point, and the
  reparameterization trick (`z = mu + sigma * epsilon`) makes sampling differentiable so gradients
  can flow through it.
- The ELBO loss combines a reconstruction term with a closed-form KL-divergence term that
  regularizes the latent distribution toward a standard normal prior.
- Minimizing the (negative) ELBO is the VAE's full training objective — no additional
  from-scratch backpropagation derivation is needed beyond what the prerequisite course already
  covered.

**You should now be able to:** implement the reparameterization trick and the ELBO loss; train a
VAE on an image dataset and sample new images from its decoder.

**Next week:** generative models II — Generative Adversarial Networks.

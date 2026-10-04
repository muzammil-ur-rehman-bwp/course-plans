# Week 10 Summary — Autoencoders

**Key takeaways:**
- An autoencoder learns to reconstruct its input through an encoder-decoder pair with a bottleneck
  (latent) layer, trained without labels.
- The bottleneck layer gives a learned low-dimensional representation usable for dimensionality
  reduction or visualization.
- Denoising autoencoders reconstruct a clean target from a deliberately corrupted input, learning
  more robust representations than plain reconstruction.
- A linear autoencoder trained with squared-error loss learns a subspace spanned by the same
  directions as PCA's top principal components; non-linear activations let it capture structure
  PCA cannot.

**You should now be able to:** build and train a (denoising) autoencoder in PyTorch; explain the
linear-autoencoder-to-PCA connection; visualize reconstructions and a learned latent space.

**Next week:** generative models I — Variational Autoencoders, the reparameterization trick, and
the ELBO.

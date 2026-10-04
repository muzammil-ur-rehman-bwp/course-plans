# Week 12 Summary — Generative Models II: Generative Adversarial Networks

**Key takeaways:**
- A GAN trains a generator (noise → data-like samples) and a discriminator (real vs. fake
  classifier) against each other in a minimax game.
- Training alternates discriminator and generator updates; neither network's loss alone is a
  reliable progress signal, unlike a standard supervised training loop.
- Mode collapse (limited sample diversity) and training instability (oscillating/diverging losses)
  are the two most common GAN failure modes, and both are diagnosed more from sample inspection
  than from loss curves alone.
- GANs and VAEs are two different answers to the same generative modeling question raised in
  Week 11.

**You should now be able to:** implement the adversarial training loop; explain the minimax
objective; recognize mode collapse and training instability from sample/loss evidence.

**Next week:** optimization and regularization for deep nets, revisited — weight decay vs. L2
under Adam, label smoothing, mixed precision, and large-batch training.

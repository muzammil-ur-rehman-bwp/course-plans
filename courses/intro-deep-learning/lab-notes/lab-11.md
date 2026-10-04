# Lab Notes 11 — Variational Autoencoders

**Concept recap:** the reparameterization trick (`z = mu + sigma * eps`) keeps sampling
differentiable; the ELBO loss is reconstruction loss plus a closed-form Gaussian KL term, and
minimizing it is equivalent to maximizing the ELBO.

**Common pitfalls:**
- Forgetting `std = torch.exp(0.5 * logvar)` and instead treating the network's `logvar` output
  as if it were `sigma` directly — the encoder outputs *log*-variance specifically so it can take
  any real value (variance must be positive, and the exponential enforces that automatically).
- Using `reduction='mean'` instead of `reduction='sum'` for the reconstruction loss while using
  the KL term's `sum` form unchanged — this changes the relative weighting between the two terms
  and can make the KL term dominate or vanish unintentionally; keep both terms' reduction
  consistent (both summed over the batch, typically).
- Not detaching or re-sampling `eps` fresh on every forward pass — reusing a stored `eps` tensor
  across different inputs removes the stochasticity the model is supposed to have.
- In Task D's sampling, sampling `z` from the *encoder's* output distribution for a specific input
  rather than directly from the prior `N(0, I)` — generating genuinely new samples requires
  sampling from the prior, not from a particular input's posterior.

**Debugging tip:** if Task C's KL term grows without bound instead of stabilizing, check whether
the reconstruction and KL terms are balanced (a very small reconstruction-loss weight relative to
KL can cause the model to ignore reconstruction and collapse `z` toward the prior, a known VAE
pathology called "posterior collapse").

**Instructor tip:** have students plot reconstruction loss and KL loss as two separate lines, not
just the summed total — the summed loss alone hides whether a model is trading off the two terms
in a healthy way or has collapsed toward one of them.

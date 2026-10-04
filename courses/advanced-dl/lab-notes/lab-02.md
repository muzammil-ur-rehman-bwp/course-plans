# Lab Notes 2 — Toy SDE Score-Based Diffusion

**Concept recap:** the score network is trained to match the closed-form conditional score of the
VP-SDE's Gaussian marginal; the reverse-SDE sampler integrates backwards from Gaussian noise to a
data sample using the trained score.

**Common pitfalls:**
- Forgetting the sign flip in the reverse sampler — the reverse-time Euler-Maruyama update
  *subtracts* the drift term rather than adding it, since time is running backwards; a sign error
  here typically produces samples that diverge rather than denoise.
- Sampling $t$ too close to 0 or 1 without the small epsilon offset (`1e-3` in the lecture code)
  — at $t=0$ the marginal std is exactly 0, producing a division-by-zero in the target-score
  computation.
- In Task D, using too few `n_steps` values to see a clear trend, or comparing sample quality only
  visually without any quantitative signal — a simple per-component assignment-accuracy check
  (which mixture component each sample is closest to) is enough to make the trend concrete.
- Training the score network with too high a learning rate, causing loss divergence that looks
  superficially like "the model isn't learning" rather than "the optimizer is unstable" — try
  Adam with a learning rate around 1e-3 as a reasonable starting point.

**Debugging tip:** before training, verify `marginal_prob` by sampling `x_t` at `t` near 0 and
confirming it is close to `x0`, and at `t` near 1 and confirming it is close to pure noise.

**Instructor tip:** have students predict, before running Task D, whether `n_steps=10` will show
visible bias toward the mixture means or toward blurred, intermediate positions — this reinforces
reading the reverse-SDE's discretization error as a predictive consequence of the derivation, not
a black-box empirical surprise.

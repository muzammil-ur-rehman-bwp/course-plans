# Lab Notes 7 — Training and Sampling a Toy Diffusion Model

**Concept recap:** training regresses a noise-prediction network onto the actual noise added at
a randomly sampled timestep; sampling iteratively applies the learned reverse step from $t=T$
down to $t=0$.

**Common pitfalls:**
- **Diffusion model noise-schedule bugs carried over from Lab 6** — if `alpha_bars`/`betas` are
  inconsistent between the training loss and the sampling loop (e.g., recomputed differently, or
  a different $T$ used in each), training can appear to converge while sampling produces garbage,
  because the sampler's reverse-step formula assumes the *same* schedule that generated its
  training targets.
- Passing the raw integer timestep `t` into the noise predictor without normalizing it (the
  lecture's `TinyNoisePredictor` divides by 200) — an unnormalized large integer timestep can
  dominate the network's input and destabilize training.
- Forgetting the extra noise term ($+\sigma_t z$) at every reverse step except the final one
  ($t=1\to0$) — omitting it entirely (even at $t>0$) produces a valid but different (purely
  deterministic, DDIM-like) sampler, which is not a bug per se but is **not** what this week's
  lecture derived; flag it explicitly if students do this so they know what they built.
- Running the reverse loop in the wrong direction (`range(T)` instead of `reversed(range(T))`) —
  this silently runs the reverse process "backward," producing nonsense samples with no obvious
  error message.

**Debugging tip:** if Task B's generated samples look like pure noise, first check the reverse
loop's iteration direction and off-by-one indexing on `alphas[t]`/`alpha_bars[t]`/`betas[t]`
before suspecting the trained network itself.

**Instructor tip:** Task C (partial-training comparison) is valuable precisely because
under-trained diffusion samples often look like "slightly denoised noise" rather than
obviously-wrong structured output — this is a good moment to discuss why diffusion training
curves can look deceptively smooth even when sample quality is still poor.

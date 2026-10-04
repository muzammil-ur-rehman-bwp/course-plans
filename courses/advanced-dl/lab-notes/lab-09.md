# Lab Notes 9 — Speculative Decoding Simulation

**Concept recap:** the accept/reject/resample rule's correctness proof shows the output
distribution exactly equals the target model's, regardless of the draft model's quality; draft
quality only affects speed (expected tokens accepted per target forward pass).

**Common pitfalls:**
- Forgetting to renormalize the residual distribution $(p-q)_+$ before resampling — an
  unnormalized residual will not sum to 1 and `torch.multinomial` will silently misbehave or
  error depending on the implementation.
- In Task A, using too few trials to see a statistically convincing match to $p$ — 10,000 trials
  is a reasonable minimum for a 3-symbol vocabulary; fewer can produce frequency estimates noisy
  enough to look like a bug when none exists.
- Confusing "the draft model is bad" with "the procedure is incorrect" — a disagreeing draft
  model in Task C should produce *fewer* accepted tokens per block (slower), never a different
  output distribution; if Task C shows a distributional difference between the two draft
  settings, that is a bug, not an expected result.
- In Task D, describing the rollback vaguely ("reset the cache") rather than precisely stating
  that only cache entries for positions $\geq j$ need rolling back, since positions $<j$ reflect a
  prefix that remains valid regardless of what happens at or after $j$.

**Debugging tip:** as a sanity check, set $q=p$ exactly (draft perfectly matches target) in Task A
and confirm the acceptance probability is 1 for every symbol, so no resampling ever occurs.

**Instructor tip:** ask students to predict, before running Task C, the direction of the effect
(does a worse draft model change *what* gets generated, or only *how fast*?) — correctly
predicting "only how fast" before seeing the result is good evidence they have internalized
Section 3's correctness proof rather than treating speculative decoding as an approximate method.

# Lab Notes 10 — Reproducing Model-Size Double Descent

**Concept recap:** test error peaks near the interpolation threshold (effective capacity ≈ amount
needed to fit the training set exactly), then descends again as capacity grows further.

**Common pitfalls:**
- Using too strong a `ridge` term in `train_random_feature_model` — heavy regularization can
  suppress the double-descent peak entirely by preventing the model from ever truly interpolating;
  keep `ridge` small (as in the lecture-content code) to see the effect clearly.
- Not training for enough steps near the interpolation threshold specifically — the optimization
  problem is often most poorly conditioned exactly at the threshold, so a fixed step count tuned
  to work well away from the threshold may undertrain there; if the peak looks unexpectedly mild,
  try increasing `steps` near `width ≈ n_train` specifically.
- In Task D, choosing a fixed width too far from any plausible peak — if width is very large or
  very small relative to any `n_train` in the sweep range, the width-specific interpolation
  threshold may fall outside the swept range entirely and no peak will be visible.

**Debugging tip:** if no peak appears in Task B at all, first confirm the random features
(`W` in `train_random_feature_model`) are **not** retrained across widths with the same seed in a
way that accidentally correlates them with `y_train`; the lecture-content code deliberately seeds
`W` by `width` to keep it genuinely fixed and random per width.

**Instructor tip:** ask students to predict, before running Task D, whether the sample-size-axis
peak will occur at a *smaller* or *larger* `n_train` than the fixed width chosen — reinforcing
that the threshold is a relationship between capacity and data size, not a fixed number.

# Lab Notes 3 — Mean-Field Signal Propagation vs. Xavier/He

**Concept recap:** the mean-field variance recursion
$q^{l+1} = \sigma_w^2\,\mathbb E[\phi(\sqrt{q^l}z)^2] + \sigma_b^2$ generalizes Xavier/He
initialization, recovering both as fixed-point special cases.

**Common pitfalls:**
- Using too few Monte Carlo samples (`n_mc`) in `propagate_variance`, producing a noisy recursion
  that drifts from the true fixed point purely from estimation noise rather than a real scaling
  error — use at least 100,000 samples, as in the lecture-content code, for Tasks B–D.
- In Task C, forgetting that leaky-ReLU's expectation term is **not** simply $\tfrac12 q$ (that is
  specific to plain ReLU) — re-derive or numerically estimate $\mathbb E[\phi(\sqrt q z)^2]$ for
  the leaky-ReLU case directly rather than reusing the ReLU shortcut.
- In Task D, confusing the correlation recursion's fixed point with the variance recursion's —
  they are different recursions tracking different quantities, and both must be run with
  consistent $\sigma_w^2$ to be comparable.

**Debugging tip:** if your Task C fixed-point search doesn't converge, bracket $\sigma_w^2$
between the Task A linear value (1.0) and He value (2.0) — since leaky-ReLU interpolates between
these two activations as $\alpha$ ranges over $(0,1)$, its fixed-point $\sigma_w^2$ must lie in
this interval too.

**Instructor tip:** ask students to predict, before running Task D, whether two highly correlated
inputs ($c^0=0.9$) will become *more* or *less* correlated with depth under He-scaled ReLU — this
previews the "order vs. chaos" framing from the Week 3 lecture content without requiring the full
critical-$\sigma_w$ analysis.

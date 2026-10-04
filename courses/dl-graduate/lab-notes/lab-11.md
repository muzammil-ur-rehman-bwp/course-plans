# Lab Notes 11 — REINFORCE with a Baseline

**Concept recap:** REINFORCE's gradient estimator is $\sum_t \nabla_\theta\log\pi_\theta(a_t\mid
s_t)\,G_t$; subtracting an action-independent baseline leaves it unbiased while typically
reducing variance.

**Common pitfalls:**
- Computing returns $G_t$ in the **wrong order** (forward instead of the backward recursive
  accumulation in `compute_returns`) — iterating forward from $t=0$ without the
  `reversed(...)` accumulation produces incorrect (non-discounted-from-$t$) return values.
- Subtracting a baseline computed **per-batch-across-all-timesteps** in a way that accidentally
  includes the current trajectory's own returns in a way that leaks information about the action
  taken (the lecture's simple `returns.mean()` baseline is action-independent and safe; a more
  elaborate per-state baseline must be checked carefully for this property).
- Using `torch.log(probs...)` without numerical-stability guarding (the lecture adds `+1e-8`) —
  for actions whose probability has collapsed very close to 0, this can otherwise produce `-inf`
  / `NaN` losses.
- Forgetting the **minus sign** when converting "ascend $J$" into a loss to descend — the lecture
  returns `-(log_probs * returns).mean()`; dropping the negative sign silently trains the policy
  to do the *opposite* of what REINFORCE intends (minimize expected return).

**Debugging tip:** if training diverges or the policy visibly gets *worse* over time, check the
sign convention first (`-(log_probs * returns).mean()` vs. `(log_probs * returns).mean()`) before
suspecting the returns computation.

**Instructor tip:** Task C's variance-measurement exercise is conceptually the most important
part of this lab — make sure students actually compute and compare a numeric variance, not just
visually compare two noisy training curves, since training-curve noise conflates
gradient-estimator variance with other sources of randomness (environment stochasticity, network
initialization).

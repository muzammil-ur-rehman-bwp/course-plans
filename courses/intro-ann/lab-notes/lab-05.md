# Lab Notes 5 — Loss Functions and the Saturation Problem

**Concept recap:** cross-entropy's gradient with respect to the pre-activation $z$ simplifies to
$\hat y - y$ for both the binary (sigmoid) and categorical (softmax) cases, which is why it does
not suffer MSE's vanishing-gradient problem at saturation.

**Common pitfalls:**
- Forgetting `np.clip` before `np.log`, causing `log(0) = -inf` and a `nan` loss the moment a
  prediction is exactly 0 or 1 — always clip to `[eps, 1-eps]` first.
- Confusing "the loss value is small" with "the gradient is large enough to keep learning" — Task
  C exists to show these are different questions; a loss can look reasonable while its gradient
  has already vanished.
- In Task D, applying cross-entropy's one-hot sum over the wrong array axis for a batch of
  examples (summing over the batch instead of the class dimension) — for this lab's single-example
  case this is not yet an issue, but get the axis convention right now since Week 8's from-scratch
  network will batch over multiple examples.

**Debugging tip:** if a gradient-vs-prediction plot looks identical for MSE and cross-entropy,
double check you're differentiating with respect to $z$ (pre-activation), not $\hat y$
(post-activation) — differentiating with respect to $\hat y$ hides the cancellation that makes
cross-entropy's $z$-gradient special.

**Instructor tip:** Task C is the most conceptually important exercise this week — walk around
during lab and ask students to explain, in their own words, why the two gradient curves look
different, rather than letting them treat it as "just another plot to make."

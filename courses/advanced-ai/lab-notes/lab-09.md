# Lab Notes 9 — Reward Modeling from Pairwise Preferences

**Concept recap:** reward modeling fits r̂(x) = w·x by maximizing the log-likelihood of observed
pairwise preferences under a Bradley-Terry (logistic) model; it narrows the outer-alignment gap
relative to a hand-specified reward, but is bounded entirely by the coverage and consistency of
its training preference data (Week 9, §1).

**Common pitfalls:**
- Choosing a learning rate too high or too few epochs, so the logistic-regression fit has not
  actually converged — verify convergence (e.g., by checking that the log-likelihood has
  plateaued) before comparing the learned `w` to `w_true`; an under-trained fit will look like a
  "biased" result even on clean, unbiased Task B data.
- In Task C, applying the feature-based bias inconsistently — e.g., flipping
  `preferred_is_a` for the wrong subset of pairs, or applying it to the features instead of the
  label — double-check the bias is implemented as a label-flip conditioned on the specified
  feature, exactly as described, not as a change to the underlying feature values.
- Comparing `w_true` and the learned `w` by raw magnitude rather than direction — logistic
  regression on preference *differences* identifies `w` only up to a positive scale factor, so
  cosine similarity (or normalized comparison) is the correct comparison, not elementwise
  difference.
- In Task D, using too few trials per bias fraction to get a stable degradation curve — preference
  sampling is stochastic; average over multiple random seeds per bias-fraction setting if the
  curve looks noisy.

**Debugging tip:** first fit on data with *zero* noise and *zero* bias — the learned `w` should
be nearly identical in direction to `w_true` (up to scale); if it is not, the bug is in Task A or
B, not in the bias logic you will add in Task C.

**Instructor tip:** before running Task D, ask students to predict whether degradation will be
roughly linear in the bias fraction or will show a threshold effect — the Bradley-Terry
log-likelihood's sensitivity to a systematically mislabeled subset is a concrete, numerical way
to make Week 9's abstract "inherits any blind spots in the training data" warning tangible rather
than merely asserted.

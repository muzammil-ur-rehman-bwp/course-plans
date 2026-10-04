# Lab Notes 1 — Empirical Risk vs. True Risk

**Concept recap:** empirical risk is computed on the training sample; true risk is the (unknown,
in practice) expectation over the full distribution. ERM over an unrestricted hypothesis class
can drive empirical risk to exactly zero (memorization) without constraining true risk at all.

**Common pitfalls:**
- Evaluating the memorizer's "true risk" on points that overlap with the training set (e.g.,
  reusing training data as test data), which hides the gap instead of revealing it.
- Confusing "restricted hypothesis class" with "regularized hypothesis class" — Task D uses an
  unregularized but *restricted* (linear) class; regularization (Weeks 6–9) is a separate, later
  mechanism for controlling the same overfitting problem.
- Forgetting that the memorizer's default prediction for unseen inputs is itself a somewhat
  arbitrary design choice, which can slightly shift the measured "true risk" number without
  changing the qualitative conclusion.

**Debugging tip:** print the memorizer's training accuracy and test accuracy side by side first —
training accuracy should be *exactly* 1.0, not approximately 1.0; if it isn't, the lookup table
has a bug (e.g., floating-point key mismatches — use a tolerance-free representation or tuple keys
derived from exact training values only).

**Instructor tip:** this lab is designed to be almost unsettling — the memorizer "looks like" a
perfect model by every training-set metric. Use it to anchor the entire semester's framing: every
later bound (PAC, VC, Rademacher) is a formal answer to "how do I know Task C's failure mode isn't
happening to my restricted model?"

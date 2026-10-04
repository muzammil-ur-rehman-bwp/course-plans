# Lab Notes 11 — Regularization From Scratch

**Concept recap:** L2 regularization shrinks weights proportionally every step; dropout forces
redundancy across units; early stopping uses the validation curve directly as a stopping signal.

**Common pitfalls:**
- Applying dropout's inverted scaling (`/ (1 - p)`) at **test time** as well as training time —
  dropout's mask and scaling apply only during training; at test/evaluation time, use the full,
  unmodified network (set `training=False`).
- Applying the L2 penalty gradient to **biases** as well as weights — standard practice
  regularizes weight matrices only, not biases, since biases do not contribute to the
  memorization/capacity concern the penalty is meant to address.
- Setting $\lambda$ far too high in Task B, which can make training loss itself fail to decrease
  (the regularization penalty dominates the data loss) — if training loss stalls near a high
  value, the step size or $\lambda$ is likely too large, not a backprop bug.
- Needing a validation set that is too small to give a stable loss estimate — with only a handful
  of validation examples, the validation curve can be noisy enough to trigger early stopping
  prematurely; this is a real practical trade-off worth discussing, not necessarily a bug.

**Debugging tip:** verify dropout's `training=False` path first, in isolation, by confirming the
output is identical to the network with no dropout layer at all — a scaling bug here silently
corrupts every subsequent evaluation.

**Instructor tip:** Task A's visible overfitting gap is the lab's hook — make sure it is clearly
overfitting (not just noisy) before moving on, or Tasks B–D's "fixes" will not show a convincing
before/after contrast.

# Lab Notes 5 — Batch Normalization and Layer Normalization

**Concept recap:** BatchNorm uses the **current batch's** mean/variance at training time but
**running (accumulated) statistics** at evaluation time; LayerNorm uses only the current
example's own features at both times, with no batch dependence at all.

**Common pitfalls:**
- **The single most common bug in this lab:** using the current batch's mean/variance at
  evaluation time instead of the running statistics (i.e., forgetting the `training` flag's
  effect, or updating `running_mean`/`running_var` even when `training=False`). This often looks
  fine on the *same* data distribution the model trained on, and only reveals itself as a subtle,
  hard-to-diagnose accuracy drop on genuinely held-out data or small batches at inference time.
- Updating the running statistics using the exponential-moving-average formula but with the
  `momentum` term's roles swapped (i.e., writing
  `momentum*batch_stat + (1-momentum)*running_stat` instead of the reverse) — this still runs and
  still "sort of works" for high-momentum values, making it an easy bug to miss.
- Forgetting that `self.x_hat` and `self.std_inv` (or equivalent cached forward-pass values) must
  be **recomputed on every forward call**, not reused from a previous batch, when implementing
  backward — a stale cache silently uses the wrong batch's statistics in the gradient formula.
- In Task C, implementing LayerNorm by copying BatchNorm's code and only renaming variables
  without changing the **axis** the mean/variance are computed over — this is the single-line
  bug that makes a "LayerNorm" implementation secretly still be BatchNorm.

**Debugging tip:** if Task B's eval-mode output does not differ from training-mode output, check
first whether `training=False` is actually being passed through to the `forward` call, then
whether running statistics were ever updated at all during training.

**Instructor tip:** Task C's batch-size-1 check is the fastest way to catch a LayerNorm
implementation that is secretly BatchNorm — if two students implement it correctly, their
batch-size-1 and batch-size-32 outputs for the same example should be *identical*; if their
BatchNorm implementation is compared the same way, it will *not* be identical (and often degenerate
at batch size 1), which is the pedagogical point.

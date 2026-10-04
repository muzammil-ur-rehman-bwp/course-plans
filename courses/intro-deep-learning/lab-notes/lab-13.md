# Lab Notes 13 — AdamW, Label Smoothing, and Mixed Precision

**Concept recap:** `AdamW`'s `weight_decay` is applied directly to the weights, decoupled from the
adaptive gradient scaling that `Adam`'s `weight_decay` (an L2 penalty folded into the gradient) is
subject to; both are valid PyTorch arguments with the same name but different underlying
behavior.

**Common pitfalls:**
- Assuming `Adam(weight_decay=x)` and `AdamW(weight_decay=x)` with the same `x` must produce
  the same trained model, since they share an argument name — Week 13's lecture explains
  precisely why they do not; Task A is designed to make this difference empirically visible, not
  just theoretically asserted.
- Setting `label_smoothing` too high (e.g., above ~0.2) for a small number of classes, which can
  noticeably hurt accuracy by over-softening targets — the lecture's `0.1` default is a
  reasonable starting point, not a value to push aggressively without checking validation
  accuracy.
- On CPU-only environments, attempting to actually run `torch.cuda.amp.autocast()`/`GradScaler`,
  which require a CUDA GPU — Task C explicitly allows (and expects) a conceptual walkthrough
  instead when no GPU is available; do not treat this as a failed task.
- In Task D, scaling the learning rate linearly with batch size but forgetting to also keep (or
  add) the warmup from Week 5 — large-batch training without warmup is exactly the unstable
  scenario Week 13's lecture warns about.

**Debugging tip:** if Task A shows no meaningful difference between `Adam` and `AdamW`, check
that `weight_decay` is set to a non-trivial value (e.g., `1e-4` or larger) — at `weight_decay=0`
the two optimizers are equivalent by construction, since there is no decay term to decouple.

**Instructor tip:** Task A is this lab's most conceptually important result — if time is limited,
prioritize it over Tasks C/D, since it directly tests the lecture's core theoretical claim.

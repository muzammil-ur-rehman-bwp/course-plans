# Lab Notes 6 — Gradient Descent Variants

**Concept recap:** all three gradient descent variants use the same update rule; they differ only
in how many examples contribute to each gradient estimate before a weight update is applied.

**Common pitfalls:**
- Choosing a learning rate that works for batch gradient descent but is far too large for SGD —
  SGD's noisier, per-example gradients typically need a smaller learning rate to avoid diverging;
  if Task C's SGD curve is wildly unstable, try lowering its learning rate before suspecting a
  bug.
- Forgetting to re-shuffle every epoch (shuffling once before the first epoch, then reusing the
  same order) — this is a subtle version of Task D's ablation happening by accident instead of on
  purpose; re-shuffle inside the epoch loop, not outside it.
- Off-by-one slicing when the dataset size is not an exact multiple of the batch size — the final,
  smaller batch is still valid and should not be dropped or cause an index error.
- Not normalizing/centering the synthetic `x` values — on some random seeds this makes the loss
  surface's curvature very different along each parameter's axis, requiring a much smaller
  learning rate than expected; this foreshadows Week 9's discussion of why input scaling matters.

**Debugging tip:** for Task C, plot the loss on a log scale if batch gradient descent's loss drops
so fast it's hard to compare against the noisier SGD/mini-batch curves on a linear scale.

**Instructor tip:** Task D is this week's conceptual payoff — have students articulate, not just
observe, why sorted (unshuffled) mini-batches produce a worse or more erratic loss curve.

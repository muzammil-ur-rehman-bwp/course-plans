# Lab Notes 5 — Feature Learning vs. Kernel Regression Across Width

**Concept recap:** the gap between kernel-regression-at-initialization predictions and actually-
trained-network predictions is a direct empirical signature of feature learning; it should shrink
as width grows toward the NTK limit.

**Common pitfalls:**
- Using too small a ridge term in `kernel_regression_predict`, making the kernel-regression
  solution numerically unstable (especially at smaller widths, where the empirical NTK can be
  poorly conditioned) and producing misleadingly large kernel-regression MSE unrelated to the
  actual lazy-vs-feature-learning comparison.
- Training for too few steps at larger widths before comparing test MSE — an undertrained large
  width network can show an artificially large gap that reflects undertraining, not a genuine
  feature-learning effect; confirm training loss has plateaued at every width before comparing.
- In Task D, applying the linear probe to post-training activations only — the comparison that
  matters is *pre-training vs. post-training* probe accuracy at the same width, to show the
  representation itself has changed, not merely that a trained readout exists.

**Debugging tip:** if kernel-regression MSE is unexpectedly worse than a trivial constant-
prediction baseline, check the ridge regularization value first — this is the most common cause at
small widths.

**Instructor tip:** ask students, before running Task D, to predict which width will show a larger
probe-accuracy improvement from training, and have them justify the prediction using Week 5 §2's
mechanisms (finite width, parameterization, learning rate) rather than intuition alone.

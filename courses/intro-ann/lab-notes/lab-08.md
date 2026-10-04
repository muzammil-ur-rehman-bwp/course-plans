# Lab Notes 8 — A Complete From-Scratch Network, Trained End-to-End

**Concept recap:** the `NeuralNetwork` class is Week 7's derivation, batched and wrapped in a
training loop; gradient checking (Week 7) should be used *before* trusting a long training run,
not after a confusing failure.

**Common pitfalls:**
- Forgetting to average gradients by the batch size `m` inside `backward`, which effectively
  scales the learning rate by the batch size and can cause divergence on larger batches/datasets
  even with a learning rate that worked fine on the 4-example XOR set.
- Initializing weights to zero (or forgetting to initialize them randomly) — this makes every
  hidden unit compute the identical gradient by symmetry, so the network trains as if it had only
  one hidden unit; this is previewed here and formalized in Week 9's initialization discussion.
- Using too high a learning rate on the Task C dataset (which has more, noisier examples than
  XOR) and seeing the loss diverge to `nan` — lower the learning rate before suspecting a logic
  bug in `backward`.
- Plotting the loss curve on too few epochs to see whether it has actually converged — extend the
  epoch count if the curve is still visibly decreasing at the final epoch.

**Debugging tip:** always run the Task D gradient check on a *small* batch before launching a
full training run on Task C's larger dataset — a bug that produces a slowly-decreasing-but-wrong
loss curve is far harder to catch by eye than a gradient check that fails loudly and immediately.

**Instructor tip:** this lab is the semester's biggest payoff so far — make sure students leave
with a working, understood, from-scratch network before the midterm, since Weeks 9–11 all build
directly on this exact class (initialization, optimizers, regularization).

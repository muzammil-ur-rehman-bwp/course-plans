# Lab Notes 12 — Training an MLP on MNIST with a Framework

**Concept recap:** the framework training loop performs exactly the forward/backward/update cycle
built by hand in Weeks 8–10; the new work this week is framework mechanics (data loaders, model
classes, device/dtype handling), not new training theory.

**Common pitfalls:**
- Forgetting `optimizer.zero_grad()` inside the batch loop, silently accumulating gradients
  across batches — if loss explodes or behaves erratically after a few batches, check this first.
- Flattening image inputs incorrectly (e.g., leaving a $(batch, 1, 28, 28)$ tensor unflattened
  when feeding it into a `nn.Linear` layer that expects $(batch, 784)$) — causing a shape-mismatch
  error that is really a data-preprocessing bug, not a model bug.
- Not normalizing pixel values (left in $[0,255]$ instead of scaled to $[0,1]$ or standardized) —
  this often still "trains" but much more slowly and with a less stable loss curve, echoing the
  input-scaling lesson implicit since Week 6.
- Forgetting `model.eval()` before evaluation when dropout/batchnorm-like layers are present —
  without it, evaluation uses training-time random behavior, giving noisy, non-reproducible
  accuracy numbers.
- Evaluating accuracy on the training set and reporting it as if it were test performance — keep
  the three splits (train/validation/test) and their purposes (Week 9's course-long lesson on
  train/test discipline) straight.

**Debugging tip:** before trusting a multi-epoch run, train for just 1 epoch on a tiny subset
(e.g., 200 examples) first — if loss does not decrease noticeably even on a tiny, easy subset,
the bug is in the model/training loop, not the optimizer or data scale.

**Instructor tip:** this lab is often the first time students see a loss curve "just work" with
very little code — use that moment to explicitly connect the brevity back to everything built by
hand in Weeks 1–11, so the framework is appreciated as a tool, not mistaken for magic.

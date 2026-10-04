# Lab Notes 13 — Iterative Magnitude Pruning and the Lottery Ticket Control

**Concept recap:** the winning-ticket procedure's defining step is resetting surviving weights to
their **original** initialization values ($\theta_0$), not to their trained values and not to
fresh random values; the random-reinitialization control changes exactly one thing (the starting
values) to isolate initialization's causal role.

**Common pitfalls:**
- Forgetting to apply the pruning mask **during** retraining (only applying it once, before
  training starts) — without re-masking after every optimizer step, small gradient updates can
  make pruned weights drift away from exactly zero, silently un-pruning the network over the
  course of training.
- Confusing the winning-ticket and random-control conditions — both must use the **exact same
  sparse connectivity mask**; only the surviving weights' *initial values* differ between them.
  Using two independently-generated masks invalidates the comparison entirely.
- In Task B, comparing one-shot and iterative pruning at *different* final sparsities by
  accident (e.g., off-by-one-round arithmetic in the iterative schedule) — always verify both
  procedures reach the same final sparsity before comparing accuracy.
- In Task C, using the same temperature for computing the teacher's distillation targets and for
  computing the student's final test-time predictions — temperature scaling is a training-time
  device for softening targets; test-time predictions should use the standard (temperature=1)
  softmax/argmax.

**Debugging tip:** if the winning ticket and random control show nearly identical accuracy
(no gap at all), first check that masking is being re-applied after every optimizer step —
without this, both conditions tend to converge to similarly-performing dense-like solutions, which
would wash out exactly the effect this lab is designed to demonstrate.

**Instructor tip:** make the mask-must-be-identical-across-conditions requirement explicit and
checked before students run Task A — it is the single easiest way to invalidate this lab's entire
comparison without being obviously wrong at a glance.

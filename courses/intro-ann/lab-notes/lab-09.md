# Lab Notes 9 — Weight Initialization Experiments

**Concept recap:** zero initialization keeps all units in a layer identical forever (symmetry
problem); Xavier/He initialization choose a random scale designed to keep activation variance
roughly stable across depth, matched to the layer's activation function.

**Common pitfalls:**
- Initializing *biases* to zero is fine and standard (bias symmetry is not an issue since biases
  do not multiply the input) — only *weight* symmetry across units within a layer is the problem;
  don't randomize biases unnecessarily in Task B/C.
- Confusing "Task A's loss not decreasing" with a bug — like Week 2's XOR-on-a-single-perceptron
  result, this is the correct, expected outcome demonstrating the symmetry argument, not something
  to fix.
- Applying He initialization's variance formula to a sigmoid-activated layer (or vice versa) in
  Task C and being confused by the mismatch — the two schemes are tuned to different activation
  functions; apply each consistently with the activation it was derived for when comparing.
- Measuring activation statistics (Task C) on a single input example instead of a batch, which
  gives a noisy, unrepresentative mean/std — use a reasonably sized random batch (e.g., 100
  examples).

**Debugging tip:** if Task C's activation standard deviations shrink toward zero by the final
layer under one initializer, that is a *directly observed* instance of the vanishing-gradient
precondition discussed in lecture — point this out explicitly rather than treating it as just a
number to report.

**Instructor tip:** Task A, run right after the midterm, is a satisfying, fast demonstration —
use it to re-energize the room before the heavier Xavier/He material.

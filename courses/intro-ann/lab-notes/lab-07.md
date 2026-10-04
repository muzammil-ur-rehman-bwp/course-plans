# Lab Notes 7 — Verifying Backpropagation with Finite Differences

**Concept recap:** gradient checking compares an analytic gradient (from backpropagation) against
a numerical approximation (finite differences) computed independently from the loss function
alone — if they disagree, the analytic implementation has a bug, since the finite-difference
approximation does not depend on the backward-pass code at all.

**Common pitfalls:**
- Forgetting to **transpose** correctly in $\partial L/\partial W^{(l)} = \delta^{(l)}(a^{(l-1)})^
  \top$ — a very common from-scratch backprop bug is computing `a_prev @ delta` instead of
  `delta @ a_prev.T` (or vice versa), which often still runs without a shape error if dimensions
  happen to coincide, but produces a numerically wrong gradient every time.
- Reusing a *stale* forward pass when perturbing a parameter for the finite-difference check —
  each perturbed evaluation of $L(\theta+\epsilon)$ requires a *fresh* forward pass with that one
  parameter changed, not a cached value.
- Perturbing the wrong element of a weight matrix (e.g., always `W[0,0]` due to a copy-paste loop
  bug) — looping over every element of every parameter array, not just the first one, is required
  for Task C to actually test the whole gradient, not one entry of it repeatedly.
- Choosing $\epsilon$ too large (loses accuracy to the approximation's own curvature error) or too
  small (loses accuracy to floating-point rounding) — Task D exists to make both failure modes
  visible side by side.

**Debugging tip:** if only *one* layer's gradients fail the check while the other layer's pass,
suspect the transpose/shape bug in that specific layer's weight-gradient line first — it is the
single most common source of a partially-correct backprop implementation.

**Instructor tip:** gradient checking is the single most valuable debugging skill introduced this
semester — tell students explicitly that every from-scratch network they build for the rest of the
course (Week 8 onward) should be checked this way before being trusted.

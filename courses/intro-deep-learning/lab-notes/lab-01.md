# Lab Notes 1 — PyTorch Tensors, Autograd, and the Training Loop

**Concept recap:** `requires_grad=True` tells autograd to track operations on a tensor;
`.backward()` computes gradients via the chain rule, exactly as derived by hand in the
prerequisite course; `optimizer.zero_grad()` must be called before each `.backward()` or
gradients accumulate across steps.

**Common pitfalls:**
- Forgetting `optimizer.zero_grad()`, causing gradients to accumulate silently across steps and
  produce a training loss curve that behaves erratically.
- Calling `.backward()` on a non-scalar tensor without specifying a `gradient` argument — reduce
  to a scalar (e.g., with `.sum()` or a loss function) first.
- Comparing `.grad` against a hand-derived gradient computed with different numeric inputs than
  the ones actually used in the PyTorch expression — always print and double-check the exact
  input values first.
- Mixing up `nn.Linear(in_features, out_features)`'s argument order, leading to a confusing shape
  mismatch error several lines later rather than at the point of the actual mistake.

**Debugging tip:** when `.grad` does not match a hand-derived value, print every intermediate
tensor (`x`, `z`, `y`, `loss`) and recompute the hand derivation using those exact printed values
— most mismatches come from a numeric slip, not a conceptual one.

**Instructor tip:** Task D's NumPy cross-check is this lab's most valuable exercise for building
trust in the framework — do not let students skip it even if Tasks A–C already "worked."

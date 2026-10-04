# Lab Notes 2 — The Perceptron and the XOR Limitation

**Concept recap:** the perceptron learning rule nudges weights toward correctly classifying each
misclassified example; it provably converges on linearly separable data and provably fails to
converge on XOR.

**Common pitfalls:**
- Using a learning rate so high that weights oscillate wildly even on separable data (AND/OR) —
  if OR does not converge within a few epochs, check the learning rate before suspecting a logic
  bug.
- Forgetting to shuffle or vary the order of training examples across epochs; for a tiny 4-row
  dataset this rarely matters, but it is worth noting now since it becomes important for
  stochastic gradient descent in Week 6.
- Mistaking "my XOR plot is not decreasing to zero" for a bug to fix — it is the correct,
  expected outcome for Task D, not an error in the implementation.
- Comparing `z >= 0` vs. `z > 0` inconsistently between training and prediction — use the same
  threshold convention in both functions.

**Debugging tip:** print `w`, `b`, and the misclassification count after every epoch (not just at
the end) while developing Task C/D — a convergence curve that never prints helpful intermediate
state is much harder to debug than one that does.

**Instructor tip:** Task D is the pedagogical centerpiece of this lab. Make sure students can
articulate, in their own words, *why* it fails (no separating line exists) rather than just
observing that it fails.

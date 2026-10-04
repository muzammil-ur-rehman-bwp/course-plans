# Lab Notes 3 — Activation Functions and Their Derivatives

**Concept recap:** every activation function pairs a forward formula with a derivative used later
in backpropagation; saturating functions (sigmoid, tanh) have derivatives that vanish for large
$|z|$, while ReLU's derivative is a clean step function.

**Common pitfalls:**
- Computing `sigmoid_grad` as `sigmoid(z) * (1 - sigmoid(z))` but accidentally passing an already-
  sigmoid-transformed value in as `z` elsewhere in the notebook — keep clear which variable holds
  raw pre-activation values (`z`) vs. activated values (`a`); this distinction becomes critical in
  Week 7's backpropagation derivation.
- Naive softmax (`np.exp(z) / np.sum(np.exp(z))`) silently producing `nan` for large logits —
  Task D exists specifically to make this overflow failure visible and show the max-subtraction
  fix.
- Off-by-one errors in `leaky_relu_grad` at exactly $z=0$ — any consistent convention (slope 1 or
  $\alpha$ at the boundary) is acceptable; the gradient is undefined at a single point of measure
  zero and does not matter in practice.

**Debugging tip:** when a plotted function "looks wrong," plot it over a small, printable range
(e.g., 5 points) and print the numeric values next to the plot — visual bugs are often just a
mislabeled axis or a sign error that is obvious once you see the numbers.

**Instructor tip:** spend extra time on Task C — the numeric evidence that sigmoid/tanh gradients
vanish is what makes Week 9's vanishing-gradient discussion land as a concrete, already-observed
fact rather than an abstract warning.

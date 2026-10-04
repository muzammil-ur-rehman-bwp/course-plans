# Lab Notes 14 — Forward Pass from Scratch

**Concept recap:** a perceptron = weighted sum + activation; stacking perceptrons into layers
with non-linear activations lets the network represent non-linear functions a single perceptron
cannot (e.g., XOR).

**Common pitfalls:**
- Shape mismatches between `W1`/`x`/`b1` — print `.shape` at each step when debugging a forward
  pass; this is the single most common source of bugs in this lab.
- Forgetting to subtract the max before exponentiating in `softmax`, risking numerical overflow
  for larger input values.
- Expecting a single perceptron to learn XOR — this is a conceptual checkpoint, not a bug to fix;
  the "failure" is the pedagogical point.

**Debugging tip:** build the forward pass one layer at a time in separate cells, printing the
output shape and a few values after each layer, before combining into the final function.

**Instructor tip:** Task D's hand-check is the most valuable part of this lab — don't let
students skip it; it's what converts "I used a formula" into "I understand what the formula
computes."

# Lab Notes 4 — A General Forward Pass and Solving XOR with an MLP

**Concept recap:** `forward_pass` simply loops the pattern `z = W @ a + b; a = g(z)` once per
layer; the XOR network's hand-designed weights make the hidden layer compute OR- and AND-like
decisions that the output layer then combines.

**Common pitfalls:**
- Shape mismatches when chaining layers — `W1`'s row count must equal the number of hidden units,
  and `W2`'s column count must equal that same number; print `.shape` after every layer while
  developing Task A, exactly as advised in the prior course's forward-pass lab.
- Forgetting that `step` (a hard threshold) is not differentiable, so it is fine for this week's
  hand-designed, non-trained example but cannot be used later once backpropagation (Week 7)
  requires activation derivatives.
- In Task C, using a non-identity matrix "pass-through" layer by accident (e.g., using `np.ones`
  instead of `np.eye`), which changes the output and masks whether the general function actually
  supports extra layers correctly.

**Debugging tip:** when a multi-layer network produces an unexpected output, verify each layer in
isolation first (call `forward_pass` with just that one layer's weights/biases on a known input)
before debugging the full chain.

**Instructor tip:** Task D is worth emphasizing — defensive shape assertions are a standard,
valuable habit for any from-scratch neural network code the class will write for the rest of the
semester, especially once backpropagation adds a second set of matrix operations to get right.

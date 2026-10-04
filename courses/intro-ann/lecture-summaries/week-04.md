# Week 4 Summary — The Multi-Layer Perceptron (MLP)

**Key takeaways:**
- An MLP chains fully-connected layers: $z^{(l)} = W^{(l)}a^{(l-1)} + b^{(l)}$, $a^{(l)} =
  g^{(l)}(z^{(l)})$, generalizing the two-layer forward pass from the prior course to arbitrary
  depth.
- A small, hand-designed 2-hidden-unit MLP solves XOR exactly, concretely resolving the limitation
  a single perceptron has (Week 2) by composing two unit-level decisions into a third.
- The Universal Approximation Theorem guarantees a sufficiently large one-hidden-layer network can
  approximate any continuous function on a bounded domain — but is an existence result only; it
  says nothing about whether training will find those weights or how large "sufficient" is.
- Depth is often a far more parameter-efficient way to gain expressive power than extreme width.

**You should now be able to:** implement a general, arbitrary-depth forward pass in NumPy; trace
and verify a small MLP's computation by hand; state precisely what the Universal Approximation
Theorem does and does not guarantee.

**Next week:** loss functions — how to quantify how wrong a network's output is, and why the loss
function must match the task and the output layer's activation.

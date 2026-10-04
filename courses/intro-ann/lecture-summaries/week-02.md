# Week 2 Summary — The Perceptron and Linear Separability

**Key takeaways:**
- A perceptron computes $z = \mathbf{w}^\top\mathbf{x} + b$ and thresholds it at zero; unlike the
  McCulloch-Pitts neuron, its weights and bias are *learned* via the perceptron learning rule
  $\mathbf{w} \leftarrow \mathbf{w} + \eta(y-\hat y)\mathbf{x}$.
- The Perceptron Convergence Theorem guarantees this rule finds a separating hyperplane in finite
  time — but only if the data is linearly separable.
- AND and OR are linearly separable and are learned correctly; XOR is not linearly separable, so
  no single perceptron can learn it, no matter how long training runs.
- This limitation — not a bug, but a fundamental property of a single linear decision boundary —
  motivates stacking perceptrons into multi-layer networks (Week 4).

**You should now be able to:** implement and train a perceptron from scratch; determine whether a
small dataset is linearly separable; explain precisely why XOR defeats a single perceptron.

**Next week:** activation functions in depth — moving beyond the perceptron's hard step function
toward the smooth, differentiable activations that multi-layer networks require.

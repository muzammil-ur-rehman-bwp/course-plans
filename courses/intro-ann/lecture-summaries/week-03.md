# Week 3 Summary — Activation Functions in Depth

**Key takeaways:**
- Stacking purely linear layers collapses to a single linear function; non-linear activations are
  what give depth its expressive power.
- Sigmoid and tanh saturate for large $|z|$, shrinking gradients — the root cause of vanishing
  gradients revisited in Week 9.
- ReLU avoids saturation for positive inputs and is the default hidden-layer choice, at the cost of
  the dying-ReLU problem, which Leaky ReLU and ELU mitigate.
- Softmax generalizes sigmoid to $K>2$ classes, converting logits into a probability distribution;
  the numerically stable implementation subtracts $\max(z)$ before exponentiating.

**You should now be able to:** implement and plot each activation function and its derivative;
choose an appropriate activation for a hidden layer vs. a given output task.

**Next week:** the multi-layer perceptron — composing layers of these non-linear units and
computing a forward pass through the full network in matrix form.

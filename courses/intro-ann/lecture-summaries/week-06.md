# Week 6 Summary — Gradient Descent

**Key takeaways:**
- The gradient $\nabla L(\theta)$ points toward steepest increase; gradient descent repeatedly
  steps in the opposite direction, $\theta \leftarrow \theta - \eta \nabla L(\theta)$.
- Learning rate $\eta$ governs a speed/stability trade-off: too small converges slowly, too large
  can diverge or oscillate — for a simple quadratic this divergence threshold can be derived
  exactly.
- Batch gradient descent uses the exact gradient over all data (slow per update); SGD uses one
  example (fast, noisy); mini-batch gradient descent balances the two and is the standard choice.
- Re-shuffling data every epoch is required for mini-batch/SGD updates to be unbiased samples of
  the true gradient.

**You should now be able to:** implement gradient descent and mini-batch gradient descent with
shuffling; reason about learning-rate choice; explain the batch/stochastic/mini-batch trade-off.

**Next week:** backpropagation — deriving, via the chain rule, the gradient of the loss with
respect to every weight in a multi-layer network, so that gradient descent can be applied to
networks, not just toy scalar losses.

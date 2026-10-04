# Week 2 Summary — Universal Approximation and Expressivity

**Key takeaways:**
- The Universal Approximation Theorem guarantees a single-hidden-layer sigmoidal network can
  approximate any continuous function on a compact domain arbitrarily well — an existence result
  proved via sums of localized "bump" functions built from shifted sigmoids.
- The theorem gives no bound on required width, no guarantee gradient descent finds the weights,
  and no generalization guarantee.
- Depth-separation results show some functions need exponentially more units in a shallow network
  than in a deep one of comparable total size — depth buys expressivity width alone cannot match.

**You should now be able to:** state the theorem and its proof sketch; explain precisely what it
does and does not guarantee; explain the depth-vs-width tradeoff conceptually.

**Next week:** automatic differentiation — computational graphs, forward- vs. reverse-mode AD,
and backpropagation as a special case of the latter.

# Week 7 Summary — Backpropagation Derivation

**Key takeaways:**
- Backpropagation is the chain rule, applied once per layer, backward from the loss, reusing each
  layer's $\delta$ to compute the next layer's instead of recomputing derivatives from scratch.
- The output layer's $\delta^{(2)} = \hat y - y$ for cross-entropy + sigmoid/softmax — the clean
  result derived in Week 5 is exactly where backpropagation starts.
- The general recursive rule, $\delta^{(l)} = (W^{(l+1)\top}\delta^{(l+1)}) \odot g'(z^{(l)})$,
  projects the next layer's error backward through its weights, then scales by the current
  layer's activation derivative.
- Every weight/bias gradient follows directly from its layer's $\delta$:
  $\partial L/\partial W^{(l)} = \delta^{(l)}(a^{(l-1)})^\top$, $\partial L/\partial b^{(l)} =
  \delta^{(l)}$.

**You should now be able to:** derive, by hand, every gradient in a small 2-layer network given
its weights and a labeled example; state and apply the general recursive backpropagation rule.

**Next week:** translating this hand derivation directly into a full NumPy implementation,
trained end-to-end on a toy dataset without any framework.

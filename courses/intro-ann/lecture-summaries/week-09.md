# Week 9 Summary — Midterm + Weight Initialization & Vanishing/Exploding Gradients

**Key takeaways:**
- Identical (e.g., all-zero) weight initialization causes every unit in a layer to receive
  identical gradients forever — the symmetry problem — so weights must be initialized randomly.
- Xavier/Glorot initialization (scaled for sigmoid/tanh) and He initialization (scaled for ReLU,
  with double the variance to compensate for zeroed negative inputs) choose the random scale so
  that activation variance stays roughly stable across layers.
- Backpropagation's gradient is a product of per-layer factors across depth; if those factors are
  typically below 1 in magnitude, gradients vanish exponentially with depth; if above 1, they
  explode — both initialization and activation choice jointly influence which regime a network
  falls into.

**You should now be able to:** explain the symmetry problem and why it requires random
initialization; apply Xavier/He initialization formulas; explain conceptually why vanishing and
exploding gradients occur in deep networks.

**Next week:** optimizers — momentum, RMSProp, and Adam — which further address difficult
gradient landscapes beyond what initialization alone can fix.

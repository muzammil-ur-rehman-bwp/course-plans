# Week 5 Summary — Normalization Theory

**Key takeaways:**
- Batch Normalization normalizes per-feature using batch mean/variance at training time and
  running statistics at evaluation time; its backward pass must chain-rule through both batch
  statistics, not just the normalized value.
- The loss-landscape-smoothing explanation (BatchNorm bounds the Lipschitz constant of the loss
  with respect to activations) is better supported than the original internal-covariate-shift
  explanation.
- Layer Normalization normalizes per-example across features, with no batch-size dependence,
  making it the standard choice for sequence models and small/variable batch sizes.

**You should now be able to:** derive and implement BatchNorm's forward and backward pass from
scratch, and explain when LayerNorm is preferred over BatchNorm.

**Next week:** optimization-landscape theory I — saddle points, the Hessian, and why second-order
methods are theoretically appealing but impractical at scale.

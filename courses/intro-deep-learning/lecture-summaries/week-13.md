# Week 13 Summary — Optimization and Regularization for Deep Nets, Revisited

**Key takeaways:**
- Weight decay and L2 regularization are mathematically identical under plain SGD but diverge
  under Adam, because Adam's adaptive per-parameter scaling is applied to an L2 penalty folded
  into the gradient; AdamW decouples weight decay from that adaptive scaling.
- Label smoothing softens one-hot targets, discouraging the overconfidence that plain
  cross-entropy with hard targets can produce.
- Mixed-precision training uses lower-precision (float16/bfloat16) compute with a float32 master
  copy of the weights and loss scaling to preserve numerical stability while speeding up training.
- Large-batch training benefits from linearly scaling the learning rate with batch size, and
  Week 5's warmup becomes more important as batch size grows.

**You should now be able to:** explain the AdamW vs. Adam+L2 distinction; apply label smoothing;
describe a mixed-precision training loop and large-batch LR scaling conceptually.

**Next week:** practical deep learning workflows — end-to-end transfer learning, saving/loading/
exporting models, and debugging networks that won't train.

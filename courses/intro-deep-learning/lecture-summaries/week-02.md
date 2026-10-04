# Week 2 Summary — Deep Networks in Practice: Initialization, Normalization, Gradient Flow

**Key takeaways:**
- Xavier/Glorot initialization preserves activation variance for symmetric activations; He
  initialization does the same for ReLU, accounting for the fraction of units it zeroes out.
- Batch normalization normalizes activations using batch statistics during training and running
  statistics during evaluation — `model.train()`/`model.eval()` controls which is used.
- Dropout, revisited, is both a regularizer and an implicit ensembling method: a trained network
  behaves like an average over many sub-networks.
- Vanishing/exploding gradients are mitigated in practice by good initialization, normalization,
  gradient clipping, and (previewed) skip connections.

**You should now be able to:** apply Xavier/He initialization and batch normalization correctly;
explain dropout's dual role; name concrete mitigations for vanishing/exploding gradients.

**Next week:** convolutional neural networks I — convolution arithmetic, pooling, and parameter
sharing in depth.

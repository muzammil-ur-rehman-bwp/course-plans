# Week 2 Summary — Advanced CNN Architectures

**Key takeaways:**
- ResNet reframes a block's target as a residual $F(x)=H(x)-x$, computing $H(x)=F(x)+x$; this
  makes identity easy to learn and gives gradients a direct additive path back through the
  shortcut.
- DenseNet concatenates (rather than adds) every preceding layer's features within a dense block,
  encouraging feature reuse and short gradient paths.
- Depthwise separable convolutions factor a standard convolution into a per-channel depthwise
  step and a channel-mixing pointwise step, cutting cost by roughly $1/C_{out}+1/k^2$.

**You should now be able to:** implement a residual block, a dense block, and a depthwise
separable convolution block in PyTorch, and compute/compare their parameter and FLOP costs.

**Next week:** the Transformer architecture from scratch — scaled dot-product attention,
multi-head attention, positional encoding, and the full encoder-decoder architecture.

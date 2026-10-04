# Week 3 Summary — The Transformer Architecture From Scratch

**Key takeaways:**
- Scaled dot-product attention is $\text{softmax}(QK^\top/\sqrt{d_k})V$; the $1/\sqrt{d_k}$
  scaling prevents softmax saturation as $d_k$ grows.
- Multi-head attention runs attention in parallel across several learned low-dimensional
  projections, then concatenates and projects back.
- Sinusoidal positional encoding injects order information additively; its angle-addition
  structure lets relative position be recovered via a fixed linear transform.
- Pre-LN placement (`LayerNorm` before each sublayer) trains more stably at depth than the
  original post-LN placement.

**You should now be able to:** derive and implement scaled dot-product attention, multi-head
attention, and sinusoidal positional encoding from scratch, and assemble a full Transformer
encoder block.

**Next week:** Transformer variants — BERT-style masked language modeling, GPT-style causal
language modeling, and the Vision Transformer.

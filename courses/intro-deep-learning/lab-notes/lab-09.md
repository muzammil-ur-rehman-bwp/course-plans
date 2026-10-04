# Lab Notes 9 — Self-Attention and Multi-Head Attention

**Concept recap:** scaled dot-product attention divides scores by $\sqrt{d_k}$ before softmax;
`nn.MultiheadAttention` expects `query`, `key`, `value` arguments (which are all the same tensor
for self-attention) and, by default, expects shape `(seq, batch, embed_dim)` unless
`batch_first=True` is passed.

**Common pitfalls:**
- Forgetting the $\sqrt{d_k}$ scaling factor, which can make the softmax in Task A too "peaked"
  (close to one-hot) for larger `d_k`, producing attention weights that look qualitatively
  different from the scaled version even though the unscaled formula is otherwise correct.
- Passing `nn.MultiheadAttention` tensors in `(batch, seq, embed_dim)` order without setting
  `batch_first=True`, silently transposing batch and sequence dimensions and producing a
  confusing but still shape-valid result.
- In Task C, adding positional encoding *after* computing self-attention instead of *before* —
  positional information must be part of the input to attention, not applied to its output.
- Confusing "self-attention has no built-in order sensitivity" with "self-attention ignores
  position entirely" — it is the *weighting* that is order-agnostic without positional encoding;
  the mechanism still operates correctly, it just cannot distinguish different orderings of the
  same set of tokens.

**Debugging tip:** when Task B's attention-weight heatmaps look uniform (close to equal weight
everywhere), check whether the linear projections for Q/K/V were actually learned/initialized
distinctly, or whether $Q=K=V$ were passed through with no projection at all — truly identical
Q/K/V with no distinguishing structure can produce unhelpfully uniform attention patterns on
random/untrained weights, independent of any bug.

**Instructor tip:** Task C's shuffle experiment is the clearest way to make "self-attention is
order-agnostic without positional encoding" concrete — do not skip it even if time is short.

# Lab Notes 3 — The Transformer Encoder Block From Scratch

**Concept recap:** scaled dot-product attention is `softmax(QK^T / sqrt(d_k)) @ V`; multi-head
attention splits $Q,K,V$ into $h$ head-sized chunks, attends in parallel, and recombines;
sinusoidal positional encoding is added (not concatenated) to token embeddings.

**Common pitfalls:**
- **Attention mask shape errors:** broadcasting a `(seq_len, seq_len)` mask against a
  `(batch, heads, seq_len, seq_len)` score tensor without the two extra leading dimensions —
  PyTorch broadcasting rules mean this usually either errors or (worse) silently broadcasts
  incorrectly along the wrong axis. Always verify mask shape aligns from the *trailing* dimensions.
- **Forgetting the $1/\sqrt{d_k}$ scaling**, or applying it with the wrong $d_k$ (e.g., the full
  `d_model` instead of the per-head dimension) — this causes exactly the softmax-saturation
  problem described in lecture; a telltale symptom is attention weights that are almost all
  exactly 0 or 1, with a near-zero gradient into the attention computation.
- **Incorrect positional-encoding broadcasting:** adding a `(seq_len, d_model)` positional
  encoding to a `(batch, seq_len, d_model)` embedding tensor without an explicit batch dimension
  — this usually broadcasts correctly by luck in PyTorch, but double-check (or add an explicit
  `.unsqueeze(0)`) rather than relying on implicit broadcasting silently doing the right thing.
- Splitting heads with the wrong `.view()`/`.transpose()` order (e.g., `view(B, h, N, d_k)`
  directly without first viewing as `(B, N, h, d_k)` then transposing) — this does not raise an
  error but silently scrambles which features belong to which head.

**Debugging tip:** for Task C's relative-position check, if the numerical verification does not
come out near zero, first re-verify the positional-encoding formula's indexing (`2i` vs. `2i+1`
columns) with a tiny `d_model=4` example printed out by hand before debugging the rotation-matrix
math.

**Instructor tip:** Task A's saturation comparison is this lab's conceptual payoff — walk students
through *why* the unscaled softmax is so much more peaked, not just that it is, by having them
print the raw (unscaled) score magnitudes alongside the scaled ones.

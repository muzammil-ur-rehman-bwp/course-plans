# Lab Notes 4 — Causal Masking and a Vision Transformer Patch Pipeline

**Concept recap:** a causal mask zeroes out attention weight for any $(i,j)$ with $j>i$; MLM
masking replaces ~15% of tokens with `[MASK]` and sets other positions' labels to the
cross-entropy `ignore_index`; ViT patch embedding is a strided `Conv2d` equivalent to
per-patch linear projection.

**Common pitfalls:**
- Building the causal mask with the wrong triangular direction (`torch.triu` instead of
  `torch.tril`, or an off-by-one on the diagonal) — always explicitly check that position 0 can
  attend **only** to itself, not to position 1.
- Applying `masked_fill` with the mask's truth value inverted (filling where the mask is `True`
  instead of `False`, or vice versa) — this silently produces a model that can only attend to
  **future** tokens, which will usually still "train" (loss decreases) but is conceptually wrong
  and will not generalize to real autoregressive generation.
- In MLM masking, forgetting to set unmasked positions' labels to `-100` — this causes the loss
  to also be computed (incorrectly) on unmasked positions, diluting the actual masked-prediction
  training signal.
- Getting `PatchEmbedding`'s patch count wrong for a given `img_size`/`patch_size` combination
  that doesn't evenly divide — always assert `img_size % patch_size == 0` before computing
  `num_patches`.

**Debugging tip:** to sanity-check a causal mask, print the full boolean mask matrix for a short
sequence (5–6 tokens) and visually confirm it is lower-triangular (including the diagonal) before
plugging it into the attention function.

**Instructor tip:** Task D's comparison exercise is where most conceptual confusion between
encoder-only/decoder-only/ViT masking patterns surfaces — have students physically draw all three
mask matrices side by side rather than only describing them in words.

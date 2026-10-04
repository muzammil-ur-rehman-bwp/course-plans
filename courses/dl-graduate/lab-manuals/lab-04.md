# Lab Manual 4 — Causal Masking and a Vision Transformer Patch Pipeline

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Implement a causal attention mask and verify it blocks future-position attention; implement a
ViT-style patch-embedding pipeline.

## Setup
Create `lab04.ipynb`. Reuse Lab 3's `MultiHeadAttention`/`scaled_dot_product_attention` and the
lecture's `causal_mask` and `PatchEmbedding`.

## Procedure
1. **Task A — Causal mask:** implement `causal_mask(seq_len)`; feed it into Lab 3's attention
   function on a toy sequence and confirm (numerically, by inspecting the weight matrix) that
   position $i$ has exactly zero attention weight on every position $j>i$.
2. **Task B — MLM masking:** implement `apply_mlm_masking`; on a toy token-id tensor, confirm
   approximately 15% of positions are masked and that `labels` correctly ignores unmasked
   positions (`-100`).
3. **Task C — ViT patch embedding:** implement `PatchEmbedding` for 32×32 images with patch size
   4; confirm the output token sequence has length `num_patches + 1` and the correct `d_model`
   dimension.
4. **Task D — Comparison:** in a markdown cell, draw (as text/ASCII or an attached image) the
   attention-mask pattern for encoder-only, decoder-only, and ViT (which uses full,
   non-causal attention like an encoder) and state which pretraining objective pairs with each.

## Expected Output
A notebook with Tasks A–D; Task A's zero-attention verification printed explicitly.

## Submission
Submit `lab04.ipynb` by the end of the lab session.

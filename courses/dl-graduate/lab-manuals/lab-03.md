# Lab Manual 3 — The Transformer Encoder Block From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Implement scaled dot-product attention, multi-head attention, sinusoidal positional encoding,
and a full Transformer encoder block, entirely from scratch (no `nn.MultiheadAttention`).

## Setup
Create `lab03.ipynb`. Start from the lecture's `scaled_dot_product_attention`,
`MultiHeadAttention`, `sinusoidal_positional_encoding`, and `EncoderBlock`.

## Procedure
1. **Task A — Scaled dot-product attention:** implement it; verify on a toy 3-token example that
   attention weights sum to 1 per row and that increasing $d_k$ without scaling saturates the
   softmax (compare scaled vs. unscaled weight distributions numerically).
2. **Task B — Multi-head attention:** implement `MultiHeadAttention`; verify output shape equals
   input shape for a batch of sequences, for at least two different head counts.
3. **Task C — Positional encoding:** implement `sinusoidal_positional_encoding`; numerically
   verify the relative-position property — compute $PE_{pos+k} - R_k \cdot PE_{pos}$ for a few
   $(pos,k)$ pairs is near zero, where $R_k$ is the per-frequency rotation matrix derived in
   lecture (or verify via the angle-addition identity directly).
4. **Task D — Full encoder block:** assemble `EncoderBlock` (attention + feed-forward + residual +
   layer norm, pre-LN); run a batch of random token embeddings through 2 stacked blocks and
   confirm output shape is preserved.

## Expected Output
A notebook with Tasks A–D; Task A's saturation comparison and Task C's numerical verification
printed explicitly.

## Submission
Submit `lab03.ipynb` by the end of the lab session.

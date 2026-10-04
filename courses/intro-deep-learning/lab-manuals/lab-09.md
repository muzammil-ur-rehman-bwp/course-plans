# Lab Manual 9 — Self-Attention and Multi-Head Attention

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement scaled dot-product self-attention and a simplified multi-head version on a small
example sequence in PyTorch.

## Setup
Create `lab09.ipynb`.

## Procedure
1. **Task A — Scaled dot-product self-attention:** implement `scaled_dot_product_attention(Q, K,
   V)` as in lecture; apply it to a small provided sequence (`lab09_sequence`) with $Q=K=V$
   derived from the same input via learned linear projections; verify output shape.
2. **Task B — `nn.MultiheadAttention`:** apply `torch.nn.MultiheadAttention` to the same input
   with 2 and 4 heads; compare the resulting attention weight patterns (visualize as a heatmap).
3. **Task C — Positional encoding:** implement the sinusoidal positional encoding formula from
   lecture; add it to the input embeddings before self-attention; discuss (markdown cell) what
   changes about the attention weights when positions are shuffled, with and without positional
   encoding added.
4. **Task D — Architecture sketch:** in a markdown cell (diagram or bullet list), sketch how
   Tasks A–C would compose into one encoder layer of a Transformer, referencing the architecture
   survey from lecture.

## Expected Output
A notebook with Tasks A–D, the attention-weight heatmaps, and the Task D sketch.

## Submission
Submit `lab09.ipynb` by the end of the lab session.

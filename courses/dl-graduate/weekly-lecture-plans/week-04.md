# Week 4 Lecture Plan — Deep Learning (Graduate)
## Topic: Transformer Variants and Applications

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Analyze encoder-only (masked-language-modeling) pretraining and its attention pattern.
   (*Analyze*)
2. Analyze decoder-only causal/autoregressive pretraining and implement a causal attention mask.
   (*Apply, Analyze*)
3. Explain the Vision Transformer's patch-as-token framing. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 3's encoder block; today: how encoder-only/decoder-only/ViT variants reuse it |
| 0:15–0:40 | Encoder-only: BERT-style MLM | Random masking, bidirectional context, the MLM training signal |
| 0:40–1:05 | Decoder-only: GPT-style causal LM | The causal mask, next-token prediction, autoregressive generation |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | Vision Transformer (ViT) | Patch extraction, linear projection, class token, positional embeddings |
| 1:40–2:00 | Comparison | Side-by-side attention-masking diagrams for encoder-only, decoder-only, and encoder-decoder |

### Materials/Equipment
- Live-coding environment, PyTorch
- Slide diagrams: MLM masking pattern, causal mask matrix, ViT patch grid

### Formative Check (in-class)
Given a 5-token sequence, students draw the attention-mask matrix (which positions may attend to
which) for an encoder-only, a decoder-only, and an encoder-decoder cross-attention setting.

### Link to Lab/Assessment
Lab 4: Implementing a causal attention mask and a toy ViT patch-embedding pipeline in PyTorch
(see `lab-manuals/lab-04.md`).

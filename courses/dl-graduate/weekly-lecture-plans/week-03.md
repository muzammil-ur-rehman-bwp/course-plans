# Week 3 Lecture Plan — Deep Learning (Graduate)
## Topic: The Transformer Architecture From Scratch

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Derive scaled dot-product attention and multi-head attention from first principles. (*Analyze*)
2. Derive and implement sinusoidal positional encoding, and explain its relative-position
   property. (*Apply, Analyze*)
3. Implement a complete Transformer encoder block in PyTorch from scratch. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | The Transformer survey from the undergraduate course — diagram level only; today: full derivation |
| 0:15–0:45 | Scaled dot-product attention | $Q,K,V$; the score matrix $QK^\top$; the $1/\sqrt{d_k}$ scaling and softmax-saturation argument; worked numeric example |
| 0:45–1:05 | Multi-head attention | Per-head projections, parallel attention, concatenation, output projection |
| 1:05–1:15 | Break | — |
| 1:15–1:35 | Positional encoding | The sinusoidal formula; why it is order-agnostic-attention's fix; the relative-position linear-transform property |
| 1:35–2:00 | Encoder-decoder architecture & layer-norm placement | Sublayer composition, residual connections, cross-attention; post-LN vs. pre-LN |

### Materials/Equipment
- Live-coding environment, PyTorch
- Slide diagrams: attention score heatmap, multi-head splitting, positional-encoding sinusoid plot

### Formative Check (in-class)
For a toy $Q,K,V$ with $d_k=4$, compute the (unscaled) and scaled attention scores by hand for one
query and confirm the scaled version avoids saturating the softmax.

### Link to Lab/Assessment
Lab 3: Implementing scaled dot-product attention, multi-head attention, sinusoidal positional
encoding, and a full Transformer encoder block from scratch in PyTorch (see
`lab-manuals/lab-03.md`).

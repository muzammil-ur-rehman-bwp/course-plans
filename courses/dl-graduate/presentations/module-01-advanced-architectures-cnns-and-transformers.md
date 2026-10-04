# Presentation: Module 1 — Advanced Architectures: CNNs & Transformers (Weeks 1–4)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 1: Advanced Architectures: CNNs & Transformers
2. **Course map** — how this course extends *Introduction to Deep Learning* and builds on
   *Artificial Intelligence*, Graduate's tabular RL; scope boundary with *Artificial Neural
   Network*, Graduate
3. **Prerequisite rapid review** — CNN basics, LSTM/GRU, attention, Transformer survey, VAE/GAN,
   one slide, stated as assumed
4. **ResNet residual formulation** — $H(x)=F(x)+x$; identity-mapping argument; gradient-path
   argument
5. **DenseNet** — dense connectivity via concatenation; growth rate; feature reuse
6. **Efficiency architectures** — depthwise separable convolutions; parameter/FLOP savings
7. **Scaled dot-product attention, derived** — $QK^\top/\sqrt{d_k}$; why the scaling matters
8. **Multi-head attention** — per-head projections, parallel attention, concatenation
9. **Sinusoidal positional encoding** — the formula; the relative-position property
10. **Encoder-decoder architecture & layer-norm placement** — sublayer composition; pre-LN vs.
    post-LN
11. **Encoder-only: BERT-style MLM** — masking, bidirectional context
12. **Decoder-only: GPT-style causal LM** — the causal mask; autoregressive generation
13. **Vision Transformer (ViT)** — patches as tokens; class token; positional embeddings

**Speaker notes:** slide 7 (scaled dot-product attention, derived) is this module's conceptual
anchor — unlike the undergraduate survey, students must leave this module able to derive, not
just recognize, the Transformer's core mechanism; give the softmax-saturation argument real time
and a concrete numeric example.

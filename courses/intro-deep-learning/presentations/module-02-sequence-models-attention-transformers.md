# Presentation: Module 2 — Sequence Models, Attention & Transformers (Weeks 6–9)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 2: Sequence Models, Attention & Transformers
2. **Why vanilla RNNs struggle** — backpropagation through time, repeated multiplication
3. **The LSTM cell** — forget/input/output gates, cell state, full gate equations
4. **The GRU** — update/reset gates, comparison to LSTM
5. **Sequence-to-sequence architecture** — encoder compresses, decoder generates
6. **Teacher forcing & exposure bias** — the train/inference mismatch
7. **The seq2seq bottleneck** — one fixed-length context vector for an arbitrarily long input
8. **Attention mechanism** — scores → softmax weights → context vector
9. **Midterm review map** — Weeks 1–8 topic connections
10. **Self-attention** — scaled dot-product attention, the $\sqrt{d_k}$ scaling
11. **Multi-head attention** — parallel attention on different learned projections
12. **Positional encoding** — why order information must be added explicitly
13. **The Transformer architecture (survey)** — encoder/decoder stacks, "Attention Is All You
    Need" (Vaswani et al., 2017)

**Speaker notes:** slide 8 (attention) is the pivot of this module — frame it explicitly as
solving slide 7's bottleneck problem, so students see attention as a targeted fix, not an
unmotivated new mechanism.

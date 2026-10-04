# Presentation: Module 4 — Architectures & Evaluation: CNN, RNN, Debugging, Capstone (Weeks 13–16)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 4: Architectures & Evaluation: CNN, RNN, Debugging, Capstone
2. **Why flatten-and-MLP fails images** — spatial locality discarded
3. **The convolution operation** — kernel sliding, hand-computed example, weight sharing
4. **Pooling** — max pooling; downsampling and translation robustness
5. **A minimal CNN** — conv → pool → conv → pool → dense architecture diagram
6. **Why sequence data needs memory** — order-dependence an MLP/CNN cannot represent
7. **The RNN cell** — hidden-state update equation; unrolling through time
8. **Vanishing gradients across time** — the same root cause as Module 3's deep-network case
9. **LSTMs (conceptual)** — gated, mostly-additive cell-state update as the fix
10. **Four loss-curve patterns** — healthy / underfitting / overfitting / broken, side by side
11. **Debugging checklist** — cheapest-check-first order: labels, normalization, learning rate,
    loss/activation pairing, gradient check
12. **Hyperparameter tuning** — grid search, selected by validation performance
13. **Course map recap** — the full Weeks 1–16 pipeline diagram (reuse from Week 16 lecture
    content)
14. **Capstone presentations** — (hand off to student presentation slides, see
    `capstone-presentation-template.md`)

**Speaker notes:** slide 13 (course map recap) is the single most important slide of the
semester — give it real time, not a rushed final-minutes mention.

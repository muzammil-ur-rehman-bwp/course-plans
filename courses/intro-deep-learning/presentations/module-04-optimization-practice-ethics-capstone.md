# Presentation: Module 4 — Optimization, Practice, Ethics & Capstone (Weeks 13–16)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 4: Optimization, Practice, Ethics & Capstone
2. **Weight decay vs. L2 under Adam** — why they coincide under SGD but not under Adam
3. **AdamW** — decoupled weight decay
4. **Label smoothing** — softened targets, discouraging overconfidence
5. **Mixed-precision training (conceptual)** — float16/bfloat16 compute, float32 master weights, loss scaling
6. **Large-batch training** — linear LR scaling, warmup revisited
7. **End-to-end transfer-learning workflow** — assembling the full semester's pipeline
8. **Saving/loading/exporting models** — `state_dict`, TorchScript/ONNX (conceptual)
9. **Debugging checklist** — cheapest-check-first order: data/labels, overfit-one-batch,
   train/eval mode, gradient norms, learning rate
10. **Bias amplification** — skewed data and proxy features, worked case study
11. **Compute & environmental cost** — estimating relative training cost
12. **Current trends, grounded** — foundation models and self-supervised pretraining as
    extensions of transfer learning and the Transformer
13. **Course map recap** — the full Weeks 1–16 pipeline diagram (reuse from Week 16 lecture
    content)
14. **Capstone presentations** — (hand off to student presentation slides, see
    `capstone-presentation-template.md`)

**Speaker notes:** slide 13 (course map recap) is the single most important slide of the
semester — give it real time, not a rushed final-minutes mention.

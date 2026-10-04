# Presentation: Module 3 — Alignment Engineering and Efficient Inference (Weeks 7–9)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Deep Learning (Post Graduate): Module 3, Alignment Engineering
   and Efficient Inference
2. **PPO for RLHF** — the KL-regularized reward; the clipped surrogate objective, built on the
   graduate course's actor-critic foundation
3. **Why RLHF's PPO stage is engineering-heavy** — four interacting networks; sensitivity to
   $\beta$
4. **The closed-form optimal policy** — deriving $\pi^*\propto\pi_{\mathrm{ref}}\exp(r/\beta)$
5. **The DPO loss, derived** — implied-reward inversion; substitution into Bradley-Terry; the
   $Z(x)$ cancellation
6. **PPO-RLHF vs. DPO** — what DPO gives up for training-pipeline simplicity
7. **Quantization** — the affine PTQ mapping; the precision/accuracy tradeoff; QAT's straight-
   through estimator
8. **Knowledge distillation** — the teacher-student framework; the temperature-scaled
   distillation loss; "dark knowledge"
9. **Midterm review recap** — Weeks 1–8 central derivations checklist
10. **Speculative decoding's mechanism** — the draft/target model split; the accept/reject/
    resample rule
11. **The correctness proof** — why the output distribution is exactly the target model's,
    verified step by step
12. **KV-cache rollback** — what must be discarded on a rejection
13. **Module 3 recap** — key results checklist (DPO derivation, PTQ/QAT/distillation,
    speculative-decoding correctness)
14. **Looking ahead** — "Next: Neural Architecture Search" teaser slide

**Speaker notes:** this module's hardest derivation for students is the DPO loss's full chain
(closed-form optimal policy → implied-reward inversion → Bradley-Terry substitution → $Z(x)$
cancellation) — walk through every algebraic step on the board rather than presenting the final
loss and working backward, since Week 9's speculative-decoding correctness proof asks students to
reproduce the same "derive, then verify by direct computation" standard independently.

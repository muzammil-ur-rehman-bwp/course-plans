# Presentation: Module 2 — Sparse Architectures, In-Context Learning, and Alignment Foundations (Weeks 4–6)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Deep Learning (Post Graduate): Module 2, Sparse Architectures,
   In-Context Learning, and Alignment Foundations
2. **The Mixture-of-Experts layer** — router, softmax gate, top-$k$ sparse dispatch
3. **The decoupling argument** — parameters scale with $N$, compute scales with $k$; why this
   matters at scale
4. **Load balancing** — router collapse; the auxiliary coefficient-of-variation loss; noisy
   gating
5. **The in-context learning phenomenon** — a frozen Transformer, no weight update, task
   performance from the prompt alone
6. **Induction heads** — the two-head "complete the pattern" circuit; structural and ablation
   evidence
7. **The implicit-gradient-descent analogy** — the linear-attention/linear-regression toy
   equivalence; what it does and does not establish
8. **Evidence-grading as a transferable skill** — the checklist reused in Weeks 12–14
9. **Why pairwise human preferences** — reliability of relative judgments over absolute scores
10. **The Bradley-Terry model, derived** — the odds-ratio assumption; the reward-model logistic
    loss
11. **The RLHF pipeline** — SFT → reward model → KL-regularized RL fine-tuning, stage by stage
12. **Why the KL penalty is necessary** — reward over-optimization against an imperfect, learned
    proxy
13. **Module 2 recap** — key results checklist (MoE routing/decoupling, induction heads vs.
    implicit GD, Bradley-Terry/RLHF pipeline)
14. **Looking ahead** — "Next: PPO for RLHF and Direct Preference Optimization" teaser slide

**Speaker notes:** Week 5's evidence-grading discussion is this module's most discussion-heavy
session — budget real time for structured debate, not just lecture delivery, since the skill
(distinguishing well-supported from speculative claims) is reused explicitly in Weeks 12 and 14.

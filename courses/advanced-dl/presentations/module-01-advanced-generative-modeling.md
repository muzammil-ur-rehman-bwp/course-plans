# Presentation: Module 1 — Advanced Generative Modeling (Weeks 1–3)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Deep Learning (Post Graduate): Module 1, Advanced Generative
   Modeling
2. **Course scope recap** — this course's frontier-DL landscape vs. the graduate-DL prerequisite
   and the sibling postgraduate courses (ANN/AI/ML, Advanced)
3. **Scoping a research proposal, previewed early** — one-slide sketch of problem
   statement/survey/approach/feasibility argument, full treatment in Week 13
4. **From discrete DDPM to continuous-time SDEs** — why the continuous-time view generalizes
   the graduate course's discrete Markov chain
5. **The forward VP-SDE** — drift/diffusion coefficients; Euler-Maruyama discretization matched
   to DDPM's forward step
6. **The reverse-time SDE** — Anderson's time-reversal result; sampling reduces to knowing the
   score function
7. **Score matching, derived** — the closed-form Gaussian marginal; denoising score matching;
   the exact DDPM-loss correspondence
8. **Classifier-free guidance, derived** — the implicit-classifier substitution; the guided-score
   formula
9. **The diversity/fidelity tradeoff** — why guidance scale $w$ is a dial, not a quality knob
10. **Flow matching** — the velocity-regression objective; simulation-free, Jacobian-free
    training; contrast with maximum-likelihood CNF training
11. **Flow matching vs. the probability-flow ODE** — shared ODE-sampler structure, broader path
    family
12. **Module 1 recap** — key results checklist (VP-SDE/DDPM correspondence, score-matching-to-
    DDPM-loss correspondence, classifier-free guidance formula, flow-matching objective)
13. **Looking ahead** — "Next: Mixture-of-Experts and sparse architectures" teaser slide

**Speaker notes:** this module's hardest derivation for students is the score-matching-to-DDPM-
loss correspondence in Week 2 — budget real board time for it, since Week 3's classifier-free-
guidance derivation leans on the same "substitute, simplify, verify the correspondence" standard
this module sets for the whole course.

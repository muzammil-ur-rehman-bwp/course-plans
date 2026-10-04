# Presentation: Module 3 — Neuro-Symbolic Integration & Formal Verification (Weeks 7–9)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Knowledge Representation and Reasoning (Post Graduate): Module 3,
   Neuro-Symbolic Integration & Formal Verification
2. **Why logic needs to be differentiable** — the step-function problem; no gradient, no
   learning
3. **t-norms and t-conorms** — product, Gödel, Łukasiewicz, compared
4. **Why product wins for learning** — the gradient argument, worked numerically at a=0.9, b=0.2
5. **A differentiable soft-logic loss** — training a symbolic constraint by gradient descent,
   with no labeled examples of the rule itself
6. **Neural theorem proving (conceptual)** — soft, embedding-based unification; what is gained
   and lost
7. **Embeddings plus explicit constraints** — filtering/vetoing confidently-wrong candidates
8. **Honest assessment** — neuro-symbolic integration as an open research frontier, not a solved
   recipe
9. **Midterm review** — Weeks 1–8 checklist
10. **The model-checking problem** — Kripke structures, `M ⊨ φ`, recap of LTL
11. **The automata-theoretic approach** — Büchi automata, the product construction, accepting
    cycles
12. **Complexity** — PSPACE-complete in formula size, polynomial in model size; the real
    state-explosion bottleneck
13. **Module 3 recap** — key results checklist (product-t-norm gradient argument, the
    model-checking decision procedure, PSPACE-completeness)
14. **Looking ahead** — "Next: multi-agent belief merging and explanation research" teaser slide

**Speaker notes:** this module covers the course's two most mathematically dense topics
(differentiable-logic gradients and the automata-theoretic model-checking construction) back to
back with a midterm in between — consider splitting Week 9's lecture time generously in favor of
the model-checking material, since the midterm exam itself absorbs the review function Week 8
already allocated to Weeks 1–8 content.

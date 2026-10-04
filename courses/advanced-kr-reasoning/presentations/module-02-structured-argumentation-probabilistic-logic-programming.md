# Presentation: Module 2 — Structured Argumentation & Probabilistic Logic Programming (Weeks 5–6)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Knowledge Representation and Reasoning (Post Graduate): Module 2,
   Structured Argumentation & Probabilistic Logic Programming
2. **From opaque to structured arguments** — Dung's AF nodes vs. ASPIC+'s rules/premises/trees
3. **Strict vs. defeasible rules** — classically valid inference vs. presumptive, attackable
   inference
4. **Building an argument** — the recursive tree construction; the top rule
5. **Rebutting vs. undercutting attacks** — targeting a conclusion vs. targeting a rule's
   applicability, with the tweety/penguin worked example
6. **Preferences and defeat** — how ASPIC+ hands Dung's semantics a ready-made attack relation
7. **The distribution semantics** — probabilistic facts, total choices, induced programs
8. **Computing query probability** — `P(q) = Σ_{θ entailing q} P(θ)`, worked rains/sprinkler
   example
9. **Contrast with MLNs** — independent-fact mixture vs. weighted-formula log-linear model; no
   partition function needed
10. **Inference at scale (conceptual)** — brute-force enumeration vs. BDD compilation
11. **Module 2 recap** — key results checklist (rebut/undercut distinction, distribution-
    semantics formula, MLN contrast)
12. **Looking ahead** — "Next: neuro-symbolic integration and formal verification" teaser slide

**Speaker notes:** the rebut/undercut distinction is the single most commonly confused concept
in this module — spend real time on the tweety/penguin example specifically, and have students
state out loud, for each attack in the example, exactly which sub-argument and which element
(conclusion vs. rule) is being targeted, before moving to the distribution semantics.

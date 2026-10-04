# Presentation: Module 3 — Statistical Relational Learning & Knowledge Graphs (Weeks 9–11)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's
template).

1. **Title slide** — Knowledge Representation and Reasoning (Graduate): Module 3, Statistical
   Relational Learning & Knowledge Graphs
2. **Midterm debrief** — brief, then transition to MLNs
3. **MLNs: weighted formulas** — the (F_i, w_i) representation, grounding over a finite domain
4. **The log-linear distribution** — P(x) = (1/Z)exp(Σw_i n_i(x)), with the hard-constraint
   limiting case
5. **MLN worked grounding** — the 2-constant Friends/Smokes toy example, fully enumerated
6. **RDF triples** — subject-predicate-object, a toy knowledge graph diagram
7. **TransE** — the h+r≈t intuition, the scoring function, a 2-D illustrative diagram
8. **TransE training** — the margin-based ranking loss, corrupted triples, the training loop
9. **Link prediction** — ranking candidates for (h,r,?), a worked example
10. **Rule mining** — closed-path rules, support/confidence, the AMIE idea
11. **Neuro-symbolic reasoning over KGs** — a grounded, non-hype framing: what rules and
    embeddings each contribute and each lack
12. **Module 3 recap & looking ahead** — key results checklist (log-linear model, TransE scoring
    function, rule support/confidence); "Next: multi-agent epistemic reasoning" teaser slide

**Speaker notes:** this module is the course's clearest bridge from pure symbolic KR into
statistical/learned representations — be explicit, every week, about what is gained
(generalization) and what is lost (explainability, exactness) relative to Modules 1–2's
formalisms, since that tradeoff is Week 14's and the capstone's recurring theme.

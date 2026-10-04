# Presentation: Module 1 — Minimax Theory & High-Dimensional Statistics (Weeks 1–4)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Machine Learning (Post Graduate): Module 1, Minimax Theory &
   High-Dimensional Statistics
2. **Course scope recap** — this course's five pillars vs. the graduate-ML prerequisite and the
   sibling postgraduate courses (notably the full-information/bandit distinction vs. *Advanced
   AI*)
3. **Scoping a research proposal, previewed early** — one-slide sketch of problem
   statement/survey/approach/feasibility argument, full treatment in Week 13
4. **The minimax risk framework** — worst-case risk over a parameter class, over all estimators
5. **Fano's inequality** — statement; the information-theoretic intuition
6. **The three-step lower-bound recipe** — reduction to testing; packing-set construction;
   KL-divergence information bound
7. **Worked example: Gaussian location family** — 2-point packing, KL bound, $\Omega(1/\sqrt n)$
   rate, matched against the sample mean's upper bound
8. **Sub-Gaussian random variables** — MGF definition; bounded ⟹ sub-Gaussian (Hoeffding's lemma)
9. **Sub-exponential random variables** — why $\chi^2$-type variables need heavier tails
10. **Bernstein's inequality** — the two-regime tail bound; Chernoff-bound derivation sketch
11. **Random matrix theory basics** — why classical covariance theory breaks when $p\approx n$
12. **The Marchenko–Pastur law** — support interval; conceptual moment-method derivation sketch
13. **Consequences for PCA** — sample eigenvalues systematically distorted even under true
    covariance $=I$; the near-singularity boundary case as $\gamma\to1$
14. **Module 1 recap** — key results checklist (Fano's bound, Bernstein's two regimes,
    Marchenko–Pastur support)
15. **Looking ahead** — "Next: full-information online convex optimization and nonparametric
    Bayesian methods" teaser slide

**Speaker notes:** this module's hardest derivation for students is the Fano-inequality-based
minimax lower bound (the shift from "analyze one estimator" to "rule out all estimators" is a
genuine conceptual jump) — budget real board time for the three-step recipe, since the Week 4
Marchenko–Pastur conceptual derivation leans on the same "a deterministic limiting law exists and
can be derived" standard this module sets for the whole course.

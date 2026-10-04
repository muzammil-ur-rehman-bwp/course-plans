# Presentation: Module 2 — Online Convex Optimization & Nonparametric Bayes (Weeks 5–7)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Machine Learning (Post Graduate): Module 2, Online Convex
   Optimization & Nonparametric Bayes
2. **The full-information OCO protocol** — explicit contrast with the sibling course's bandit
   setting: the learner sees the entire loss function, not just its own realized payoff
3. **Follow-The-Regularized-Leader** — definition; the regularizer's stabilizing role
4. **Online gradient descent** — FTRL linearized; the projected-gradient update rule
5. **Deriving the regret bound** — projection non-expansiveness, telescoping, optimized $\eta$,
   $\mathrm{Regret}_T\leq DG\sqrt T$
6. **Why this setting is distinct from bandits** — gradient access vs. confidence-bound-driven
   exploration; no overlap with *Advanced AI*'s territory
7. **Motivating nonparametric Bayes** — beyond fixed-$K$ mixture models
8. **The Dirichlet process** — defining property via finite partitions; base measure and
   concentration parameter
9. **The stick-breaking construction** — Beta draws, weight construction, the weights-sum-to-1
   telescoping argument
10. **The role of $\alpha$** — small vs. large $\alpha$'s effect on cluster concentration
11. **The Chinese Restaurant Process** — the combinatorial twin of the DP; the seating rule
12. **Exchangeability** — why partition probability does not depend on arrival order, and why
    this licenses valid inference
13. **Cluster growth and DP mixtures** — $O(\alpha\log n)$ growth; building a full generative
    mixture model on the CRP
14. **Module 2 recap** — key results checklist (OGD's regret bound, stick-breaking's weight-sum
    proof, the CRP growth rate)
15. **Looking ahead** — "Next: causal inference in depth" teaser slide

**Speaker notes:** students often conflate the DP's stick-breaking (constructive) and CRP
(combinatorial) views as two different objects rather than two views of the same object —
dedicate explicit board time to walking through a small example (e.g., $n=3$ customers) both ways
side by side.

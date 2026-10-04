# Presentation: Module 2 — Multi-Agent Systems & Algorithmic Game Theory (Weeks 5–7)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Artificial Intelligence (Post Graduate): Module 2, Multi-Agent
   Systems & Algorithmic Game Theory
2. **Module 1 recap** — bridge from single-agent bandit regret to multiple simultaneously
   learning agents
3. **Independent vs. joint-action learners** — Q(a) vs. Q(a, b), diagram of what each can and
   cannot represent
4. **Non-stationarity** — why single-agent Q-learning's convergence proof breaks when the
   "environment" is itself learning
5. **Self-play** — the automatically scaling curriculum mechanism, described generically (no
   unverified system-specific claims)
6. **From defining to computing Nash equilibria** — the graduate course's definition vs. this
   course's computational question
7. **Zero-sum games** — von Neumann's minimax theorem and the linear-programming formulation
8. **PPAD-completeness** — END-OF-LINE and the parity-argument intuition; why this is a
   different kind of hardness than NP-completeness
9. **Correlated equilibria** — the polynomial-time LP-solvable relaxation; welfare comparison to
   Nash equilibrium
10. **Mechanism design** — inverting game theory: designing the rules for a desired social
    outcome
11. **The VCG mechanism** — the allocation rule and the externality payment rule; the
    dominant-strategy truthfulness proof, walked through step by step
12. **Applications** — ad auctions and resource allocation; where real systems deliberately
    depart from exact VCG, and why
13. **Module 2 recap** — key results checklist (non-stationarity's cause, PPAD-completeness,
    the VCG truthfulness proof)
14. **Looking ahead** — "Next: AI safety and alignment as a technical research area" teaser slide

**Speaker notes:** the VCG truthfulness proof (slide 11) is this module's own "prove it, don't
just state it" moment, directly mirroring Module 1's multiplicative-weights derivation — walk
through the utility decomposition on the board rather than presenting the result as a fait
accompli.

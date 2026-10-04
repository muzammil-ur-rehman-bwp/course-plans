# Presentation: Module 1 — Online Learning & Bandit Theory (Weeks 1–4)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Advanced Artificial Intelligence (Post Graduate): Module 1, Online
   Learning & Bandit Theory
2. **Course scope recap** — this course's four pillars vs. the graduate-AI prerequisite and the
   sibling postgraduate courses (ANN/ML/DL/KR&R, Advanced)
3. **Scoping a research proposal, previewed early** — one-slide sketch of problem
   statement/survey/approach/feasibility argument, full treatment in Week 13
4. **The online-learning protocol** — repeated play against an unknown loss sequence, no
   statistical assumption on how losses are generated
5. **Regret as a performance measure** — formal definition: cumulative loss vs. the best fixed
   expert in hindsight
6. **Multiplicative weights** — the multiplicative update rule and the potential-function
   intuition behind it
7. **The regret bound, derived** — Regret_T ≤ ηT + (ln N)/η, optimized to O(√(T ln N))
8. **The multi-armed bandit problem** — exploration-exploitation formalized, cumulative-regret
   objective
9. **UCB1 & the Hoeffding bound** — confidence-radius construction; the O((K log T)/Δ) regret
   bound
10. **Thompson sampling (conceptual)** — posterior sampling as an alternative to explicit
    confidence bounds
11. **Contextual bandits** — the protocol, a simplified LinUCB-style algorithm, and the bridge
    toward full RL
12. **The bandit → contextual-bandit → MDP spectrum** — state persistence and delayed reward;
    tabular Q-learning located precisely on the spectrum
13. **Module 1 recap** — key results checklist (multiplicative-weights bound, UCB1 regret bound,
    the spectrum table)
14. **Looking ahead** — "Next: multi-agent reinforcement learning and algorithmic game theory"
    teaser slide

**Speaker notes:** this module's hardest derivation for students is the multiplicative-weights
potential-function argument — budget real board time for it, since the Week 3 UCB1/Hoeffding
derivation leans on the same "derive, don't just state" standard this module sets for the whole
course.

# Presentation: Module 2 — Automated Reasoning & Rigorous Planning (Weeks 5–7)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Artificial Intelligence (Graduate): Module 2, Automated Reasoning & Rigorous Planning
2. **Module 1 recap** — one-slide bridge from game theory to constraint reasoning
3. **AC-3 and arc consistency** — algorithm diagram, pruning example
4. **Simulated annealing** — Metropolis criterion curve (illustrative), cooling schedule tradeoff
5. **Genetic algorithms** — selection/crossover/mutation diagram; no convergence guarantee, contrasted with SA
6. **CNF and the DPLL algorithm** — unit propagation + branching flowchart
7. **Clause learning (conceptual)** — conflict-driven learning diagram; why CDCL scales further
8. **SMT, briefly** — SAT + theory solver cooperation loop diagram
9. **Planning's PSPACE-completeness** — membership/hardness intuition, NP ⊆ PSPACE diagram
10. **The relaxed planning graph** — layered literal/action diagram; h_level heuristic
11. **HTN planning** — task-decomposition tree diagram
12. **Module 2 recap** — key results checklist (AC-3, DPLL soundness/completeness, planning
    PSPACE-completeness, h_level heuristic, HTN decomposition)
13. **Looking ahead** — "Next: MDPs, the Bellman equation, and the midterm" teaser slide

**Speaker notes:** the DPLL unit-propagation fixed-point loop and the relaxed-planning-graph
construction are the two places students most often under-implement a "mostly right" version;
budget extra live-coding time for both.

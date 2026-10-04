# Week 16 — Lecture Content: Capstone Research Proposal Presentations + Course Review

## 1. Capstone Research Proposal Presentations
Each student presents their final research proposal in a qualifying-exam/thesis-proposal-defense
format (problem statement, related work, proposed approach, feasibility argument or preliminary
results, anticipated risks, committee-style Q&A), following
`presentations/capstone-presentation-template.md` and graded per `assignments/capstone-rubric.md`.
This is the culminating assessment of the course's central skill: formulating and rigorously
scoping original research, not recalling or routinely applying known results.

## 2. Course Map Recap
| Pillar | Weeks | Core Results |
|---|---|---|
| Minimax theory & high-dimensional statistics | 2–4 | Minimax risk and Fano's inequality; sub-Gaussian/sub-exponential concentration and Bernstein's inequality; the Marchenko–Pastur law |
| Full-information online convex optimization | 5 | The OCO protocol; FTRL; online gradient descent's $O(\sqrt T)$ regret bound |
| Nonparametric Bayesian methods | 6–7 | The Dirichlet process and stick-breaking; the Chinese Restaurant Process and infinite mixtures |
| Causal inference in depth | 8–9 | Potential outcomes and the fundamental problem of causal inference; unconfoundedness/overlap and IPW; instrumental variables; the do-calculus rules |
| Trustworthy & robust statistical learning | 10–12 | Covariate shift and importance weighting; algorithmic-fairness impossibility results; median-of-means and trimmed-mean robust estimation |

Each pillar builds directly on a specific piece of the graduate course's assumed toolkit
(concentration inequalities generalized in Weeks 2–4; convex optimization generalized in Week 5;
Bayesian ML generalized in Weeks 6–7; the conceptual causal-inference introduction made rigorous
in Weeks 8–9; generalization theory and model-selection theory stress-tested under shift,
unfairness, and contamination in Weeks 10–12) — the single throughline of this entire course is
taking graduate-level statistical learning theory's settled tools and asking, rigorously, where
and why they break, and what provably replaces them.

## 3. Where This Course Connects Into the Sibling Postgraduate Courses
- **Advanced Artificial Intelligence:** that course's full treatment of partial-information
  (bandit) regret theory is the direct complement to this course's Week 5 full-information OCO —
  a student interested in sequential decision-making theory benefits from both, kept
  deliberately non-overlapping.
- **Advanced Artificial Neural Network / Advanced Deep Learning:** this course's high-dimensional
  statistics (Weeks 2–4) and robust-statistics (Week 12) results are the classical statistical
  foundation against which claims about overparameterized models' generalization and adversarial
  robustness should be checked — those courses study the architectures and training dynamics
  directly; this course supplies the theoretical lens.
- **Advanced Knowledge Representation and Reasoning:** this course's do-calculus treatment
  (Week 9) is a statistical/graphical take on causal structure; that course's treatment of deep
  KR formalisms is a logical/symbolic one — genuinely disjoint approaches to structured
  reasoning, each with its own place.

## 4. Closing Note
The course began (Week 1) by stating that postgraduate work is judged on the ability to formulate
and scope original research, not on recall or routine application. The capstone presentations this
week are the direct test of exactly that claim — and the skill (precise problem statements,
honest feasibility arguments, genuine engagement with what is and is not yet known) is intended to
transfer directly to any future thesis or dissertation proposal, in statistical learning theory or
otherwise.

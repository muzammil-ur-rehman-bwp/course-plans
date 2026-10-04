# Week 16 — Lecture Content: Capstone Presentations; Course Review

## 1. Capstone Presentations
Students/pairs present their research capstone in the conference-talk format specified in
`presentations/capstone-presentation-template.md`: problem statement, related work (the
literature review), method/experiment, results, limitations, and Q&A. Presentations are scored
against `assignments/capstone-rubric.md`, which weights the literature review, experimental
soundness, honest analysis of results (including limitations), the written paper, and the
presentation itself.

## 2. Course Review: The Full Map
This course moved through a deliberately bounded, disjoint slice of graduate AI:

| Phase | Weeks | Core Content |
|---|---|---|
| Graduate Foundations | 1 | Research-areas map; rigorous agent/problem formalization |
| Advanced Search & Game Theory | 2–4 | IDA*/bidirectional/SMA* search; minimax correctness; Nash equilibrium; mechanism design |
| Automated Reasoning & Rigorous Planning | 5–7 | Advanced CSP/metaheuristics; DPLL/SAT/SMT; planning complexity, heuristics, HTN |
| Decision-Theoretic AI | 8–11 | MDPs, Bellman equation, value/policy iteration, Q-learning, POMDPs, graphical-model inference |
| Complexity, Research Methods & Capstone | 12–16 | NP/PSPACE-completeness results; paper critique; current trends; the capstone itself |

Three threads run through the whole semester and are worth stating explicitly in this closing
review:
1. **Formal correctness arguments** — alpha-beta's correctness proof (Week 3), value iteration's
   contraction-mapping convergence argument (Week 8), policy iteration's finite-convergence
   argument (Week 9) — each algorithm was accompanied by *why* it is correct, not just *what* it
   does.
2. **Real complexity results, used, not just named** — SAT/CSP's NP-completeness and planning's
   PSPACE-completeness (Weeks 6, 7, 12) were used to explain *why* heuristics, approximation, and
   structure-exploitation are necessary, not treated as trivia.
3. **Research methods as a graded skill** — critiquing papers and designing sound experiments
   (Week 13) was not an afterthought; it was the explicit preparation for the capstone that just
   concluded.

## 3. Where to Go Next
This course's foundations connect directly to the sibling graduate courses for students
continuing in the AI-family sequence:
- **Artificial Neural Network (graduate):** builds on this course's optimization/search
  intuitions (e.g., gradient-based optimization is a cousin of the search/optimization ideas in
  Weeks 2 and 5) to study neural architectures and training in depth.
- **Machine Learning (graduate):** studies classical statistical ML algorithms (SVMs, ensembles)
  that this course deliberately did not cover.
- **Deep Learning (graduate):** studies deep architectures (CNNs, RNNs, Transformers) that this
  course only ever mentioned with a one-sentence pointer.
- **Knowledge Representation and Reasoning (graduate):** returns to logic and representation —
  this course's automated-reasoning unit (Week 6) is a natural bridge into that course's deeper
  treatment of description logics, frames, and semantic networks.

## 4. Closing Discussion
Open discussion: which of this semester's topics connects most directly to each student's own
capstone subtopic, and which sibling course (if any) they are most likely to take next given
their capstone experience.

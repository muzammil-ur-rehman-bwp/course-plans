# Assignment 2 — Automated Reasoning, ASP, Belief Revision, Argumentation (Weeks 5–8)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 8 |
**Due:** Start of Week 10

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Sequent calculus, 15 pts)** Construct, by hand, a full sequent-calculus derivation for
   ⊢ (p→q)→((q→r)→(p→r)) (hypothetical syllogism), explicitly naming each rule applied. Present
   the derivation in a markdown cell.
2. **(First-order tableau, 15 pts)** Construct, by hand, a first-order tableau showing
   {∀x.(P(x)→Q(x)), ∀x.(Q(x)→R(x)), P(a), ¬R(a)} is unsatisfiable, explicitly marking every
   ∀-instantiation and the final clash.
3. **(Resolution refinements, 20 pts)** Implement set-of-support resolution (Week 5) on a
   provided 7-clause unsatisfiable set (`assignment02_clauses.json`) with the negated-goal clause
   designated as the set of support. Report the number of resolvents SOS generates versus
   unrestricted resolution on the same set, and explain the difference in 2–3 sentences.
4. **(Stable models, 20 pts)** Encode a provided 5-vertex graph-coloring instance with 3 colors
   (`assignment02_graph.json`) as a normal logic program using your Week 6 stable-model checker.
   Report all valid colorings found as stable models, and verify by hand that one reported
   coloring is genuinely valid (no two adjacent vertices share a color).
5. **(AGM and argumentation, 30 pts)** (a) For a provided belief set K (as a set of propositional
   models) and a revision sentence φ, compute K∗φ using your Week 7 Dalal revision operator, and
   explicitly check postulates (K∗2), (K∗3), (K∗6) against the result. (b) For a provided
   6-argument Dung AF (`assignment02_af.json`) containing one mutual-attack pair, compute the
   grounded extension and all preferred extensions using your Week 8 implementation, and explain
   in 2–3 sentences why the grounded extension does not capture every argument a rational agent
   could accept.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

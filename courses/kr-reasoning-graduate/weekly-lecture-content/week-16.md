# Week 16 — Lecture Content: Capstone Research Presentations; Course Review

## 1. Capstone Research Presentations
Each student/pair presents their capstone in conference-talk format (problem statement, related
work, method/experiment, results with multi-seed/multi-trial honesty where applicable,
limitations, Q&A), per `presentations/capstone-presentation-template.md` and graded per
`assignments/capstone-rubric.md`. The presentation is the culmination of the literature review
and experiment work carried out since topic selection (Weeks 7–8) and the Week 15 work session.

## 2. Course Map Recap
This course's advanced-KR map, in order:
1. **Modal logic** (Week 2) — Kripke semantics, K/S4/S5, the epistemic reading.
2. **Temporal logic** (Week 3) — LTL's X/G/F/U, a look at CTL's branching-time operators.
3. **Description logics in depth** (Week 4) — the ALC tableau algorithm, PSPACE-completeness,
   OWL 2 EL/QL/RL.
4. **Automated theorem proving in depth** (Week 5) — the sequent calculus, FOL tableau,
   resolution refinements (set-of-support, ordering).
5. **Answer Set Programming** (Week 6) — the stable-model semantics via the GL-reduct.
6. **Belief revision and update** (Week 7) — the AGM postulates, Dalal revision, the
   revision/update distinction.
7. **Argumentation frameworks** (Week 8) — Dung's AF, grounded and preferred extensions.
8. **Markov Logic Networks in depth** (Week 9) — the log-linear model, grounding, inference.
9. **Knowledge graphs** (Week 10) — RDF triples, TransE embeddings, link prediction.
10. **Reasoning over knowledge graphs** (Week 11) — rule mining, grounded neuro-symbolic
    reasoning.
11. **Multi-agent epistemic reasoning** (Week 12) — common/distributed knowledge, the
    muddy-children puzzle.
12. **Ontology engineering in practice** (Week 13) — development methodology, alignment, real
    reasoner tooling.
13. **Explainability and reasoning** (Week 14) — proof trees, the symbolic-vs-ML contrast.
14. **Research methods and the capstone** (Weeks 15–16).

## 3. Connecting Back to the Undergraduate Foundations
Every week above built directly on an undergraduate-course foundation without re-deriving it:
modal/temporal logic extends propositional logic's semantics machinery; the DL tableau replaces
the undergraduate toy subsumption check with a real decision procedure; resolution refinements
extend plain resolution; ASP gives default logic's non-monotonicity a computable semantics;
argumentation and belief revision give conflicting/changing information a formal treatment
classical logic alone cannot; MLNs go deep on what was a brief survey; knowledge graphs scale up
semantic networks; multi-agent epistemic logic extends single-agent modal logic to groups;
explainability's proof trees extend the integrated agent's single-step trace to a full
derivation.

## 4. Where KR&R Research Is Headed
Closing discussion points, grounded and non-hype: large-scale knowledge graphs continue to push
symbolic representations toward statistical, embedding-based reasoning (Weeks 10–11), while
demand for trustworthy, auditable AI pushes back toward symbolic and hybrid approaches precisely
because of their explainability advantage (Week 14); neuro-symbolic integration (combining
learned components with symbolic structure, as surveyed in Week 11) remains an active, unsettled
research area rather than a solved problem — several capstone projects this semester likely
touched exactly this boundary.

## 5. Final Exam Reminder
The final exam (Week 17) is comprehensive, weighted toward Weeks 9–15 (per the Assessment Plan);
review materials will be distributed after presentations conclude.

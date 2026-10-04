# Week 16 — Lecture Content: Capstone Presentations; Course Review

## 1. Capstone Presentations
Each team presents for 5–7 minutes plus Q&A, following
`presentations/capstone-presentation-template.md`: problem statement, which two (or more) course
formalisms were integrated and why, the specific representation/implementation built, results
against the evaluation plan from the proposal, a demo if applicable, and an honest discussion of
limitations. Audience members use the Week 15 evaluation axes (correctness, scalability,
explainability) as a concrete lens for their questions and feedback.

## 2. The Course Map, Revisited
This course opened (Week 1) by arguing that knowledge representation deserves a full semester of
its own, because the brief survey in *Introduction to Artificial Intelligence* — one week each on
propositional/first-order logic, one week on STRIPS planning, one week on Bayesian networks —
cannot go deep enough to implement, trace, and genuinely understand the inference procedures
behind each formalism. Looking back across sixteen weeks, that case should now be concrete rather
than asserted:

| Phase | Weeks | What was added beyond the brief survey |
|---|---|---|
| Logic foundations | 1–4 | Normal forms, resolution refutation *in depth*, unification, FOL resolution, Skolemization |
| Representation schemes | 5–7 | A reusable rule engine; semantic networks and frames with non-monotonic inheritance; description logics, DL/FOL, OWL/RDF — none of which the brief survey covers at all |
| Constraints & non-monotonic reasoning | 8–9 | AC-3 and heuristic backtracking *in depth*; the closed-world assumption, default logic, circumscription — entirely new to this course |
| Planning in depth | 10 | Variabilized/grounded STRIPS, partial-order planning, planning graphs — beyond one week of forward search |
| Temporal/probabilistic reasoning | 11–13 | Allen's interval algebra and spatial reasoning (new); variable elimination (a level deeper than enumeration); Markov logic networks and fuzzy logic (new) |
| Practice & trends | 14–15 | An integrated, explainable knowledge-based agent; knowledge graphs and neuro-symbolic AI as current practice |

## 3. Where KR&R Research Is Headed
Two threads from Week 15 are worth restating as a closing orientation: **knowledge graphs** are
where the semantic-network/frame ideas of Weeks 6–7 operate at industrial scale today, and
**neuro-symbolic AI** is the active research question of how to combine this course's symbolic
formalisms with the statistical/learning-based pillar of AI that `Introduction to Artificial
Intelligence` surveys. Students continuing in AI — whether toward further KR&R study, machine
learning, or applied systems work — carry forward from this course a working, from-scratch
understanding of exactly how symbolic reasoning is implemented, which remains the piece most
survey-level AI courses can only describe, not build.

## 4. Closing Discussion
In the final class discussion, revisit one capstone project (volunteered by its team) and
collectively identify: which two formalisms it integrated, which weeks each came from, and one
way its design could be extended with a third formalism from the course.

# Lab Notes 15 — Structured Capstone Work Session

**Concept recap:** this session applies Week 15's critical-reading and reproducibility framework
to students' own in-progress capstone work, rather than introducing new theory.

**Common pitfalls:**
- Treating the literature review as a list of summaries rather than a *comparison* — a strong
  review states how the 3–5 papers relate to each other (agree, disagree, build on one another),
  not just what each one says in isolation.
- Scoping the reproduced/extended experiment too ambitiously for the remaining time (e.g.,
  proposing to re-derive and empirically test three separate bounds) — Task B's status check
  exists specifically to catch over-scoped projects early enough to narrow them.
- Skipping Task C's reproducibility self-check because "it's just a draft" — bad habits around
  seeds, trial counts, and unstated assumptions formed now tend to persist into the final
  submission.

**Debugging tip:** if a reproduced experiment's results don't match the original paper's reported
numbers even qualitatively, check the exact assumptions first (same data regime, same sample-size
scale, same hyperparameter ranges) before suspecting an implementation bug — many
statistical-learning-theory results are regime-dependent (recall the Week 3/15 vacuous-VC-bound
discussion) and a mismatch can be a genuine, reportable finding rather than an error.

**Instructor tip:** rotate through pairs/groups during the work session rather than giving one
long group lecture — the point of this lab is individualized, project-specific feedback, which a
single broadcast session cannot deliver.

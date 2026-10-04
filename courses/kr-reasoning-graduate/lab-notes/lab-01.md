# Lab Notes 1 — Foundations Diagnostic & Advanced-KR Landscape Mapping

**Concept recap:** this lab is a checkpoint, not new material — it confirms resolution,
unification, and rule-based forward chaining (all assumed undergraduate content) are solid before
the course builds on them starting Week 2.

**Common pitfalls:**
- Treating the diagnostic tasks as optional "review" to skim — a shaky unification implementation
  here will resurface painfully in Week 5's FOL tableau and resolution-refinement work.
- Confusing a rule engine's forward-chaining fixpoint (apply rules until nothing new derives)
  with a single pass over the rule list — a common bug that under-derives on chained rules.
- In Task C, mapping a question to "the closest-sounding week" rather than the week that
  actually owns the *technique* the question requires — read the question for its required
  formal machinery, not its surface vocabulary.

**Debugging tip:** if the Task A resolution refutation does not terminate or gives a wrong
answer, trace it on the 2-clause example from the undergraduate course's Week 2 lecture content
first — if that fails, the bug is in the resolution step itself, not in anything new this week.

**Instructor tip:** use Task C's mapping exercise to surface, out loud, which students are
genuinely unsure about an assumed topic — this is the one week built for catching that early.

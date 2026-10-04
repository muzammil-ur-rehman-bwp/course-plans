# Lab Notes 7 — Propositional Logic Inference: Resolution

**Concept recap:** resolution combines two clauses sharing a complementary literal pair into a
new clause with those literals removed; resolution refutation proves `KB ⊨ α` by negating `α`,
adding it to the KB's clauses, and showing the empty clause can be derived.

**Common pitfalls:**
- Forgetting to negate the query before adding it to the clause set — without this, resolution
  is checking the wrong thing entirely.
- Treating a clause's literal "P" and "~P" as unrelated strings instead of recognizing them as
  complements — the complement-finding logic (`literal[1:]` vs. `"~" + literal`) must be exact.
- Looping forever because the refutation function never terminates when no more new clauses can
  be derived — always check for a fixed point (no new clauses added) and return `False`.

**Debugging tip:** print every resolvent as it is derived, including which two parent clauses
produced it; if the empty clause never appears, trace by hand whether a necessary resolution
step is missing from your clause set (often: the KB was not fully converted to CNF first).

**Instructor tip:** have students first prove a resolution step **by hand** on paper (2 small
clauses) before writing any code — students who skip this step often cannot debug their own
`resolve()` function because they don't have an independent expected answer to check against.

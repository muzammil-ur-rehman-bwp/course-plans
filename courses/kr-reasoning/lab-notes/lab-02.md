# Lab Notes 2 — Normal Forms, Resolution Refutation, and SAT

**Concept recap:** CNF conversion is a fixed four-step pipeline; resolution removes a
complementary literal pair from two clauses; resolution refutation negates the query and checks
for the empty clause; SAT brute-forces every model to check satisfiability.

**Common pitfalls:**
- Forgetting to negate the query before adding it to the clause set in `resolution_refutation` —
  without this the procedure checks the wrong question entirely (whether the KB plus the query is
  consistent, not whether the KB entails the query).
- Treating `"P"` and `"~P"` as unrelated strings instead of recognizing them as complements — the
  complement-finding logic (`literal[1:]` vs. `"~" + literal`) must be exact and symmetric in both
  directions.
- Looping forever in `resolution_refutation` because the fixed-point check is missing or wrong —
  always compare the *new* resolvents against the *existing* clause set and stop when nothing new
  is derivable, returning `False`.
- In the SAT checker, mishandling the literal-truth lookup for negated literals — a common bug
  evaluates `model[lit]` directly on a string like `"~P"`, which raises a `KeyError` instead of
  correctly negating `model["P"]`.

**Debugging tip:** print every resolvent as it is derived, including which two parent clauses
produced it; cross-check a handful of these resolvents by hand before trusting the full trace.

**Instructor tip:** have students first derive one resolution step **by hand** on paper before
writing any code, and separately hand-verify satisfiability of a 3-clause set by truth table —
students who skip this cannot debug their own implementation because they have no independent
expected answer.

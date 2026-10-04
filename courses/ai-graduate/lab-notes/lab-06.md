# Lab Notes 6 — DPLL SAT Solver

**Concept recap:** DPLL alternates unit propagation (forced assignments) and branching
(guessing a value for an unassigned variable), backtracking on conflict (an empty/falsified
clause); it is sound and complete.

**Common pitfalls:**
- **Unit propagation bugs**: failing to re-scan all clauses after each propagated assignment
  (propagation can cascade — assigning one variable can create a new unit clause that was not
  unit before) — a single pass instead of a fixed-point loop is the most common bug here.
- Treating a clause with zero unassigned literals and no satisfied literal as merely "not unit"
  instead of recognizing it as a **conflict** (empty/falsified clause) that must trigger
  backtracking immediately.
- Confusing variable polarity: in the integer-literal encoding (positive int = variable,
  negative int = its negation), forgetting to compare `lit > 0` against the assigned boolean
  correctly when checking satisfaction.
- In Task C, expecting every instance near ratio 4.3 to be slow — the satisfiability threshold
  describes *average* difficulty across many random instances, not every single instance; a
  report based on one instance per ratio is anecdotal, not a real demonstration (this previews
  Week 13's point about single-run evidence).

**Debugging tip:** add a small formula with a known hand-traced solution (from the Week 6
lecture content's worked example) as a unit test that must pass before trusting the solver on
anything bigger or randomly generated.

**Instructor tip:** have students trace unit propagation's cascade by hand on a 4-clause example
where assigning one variable triggers a second, previously-non-unit clause to become unit — this
is the detail that separates a correct DPLL from one that only handles the simple case.

# Lab Notes 4 — SROIQ Fragment Satisfiability Checker

**Concept recap:** the ≥-rule creates fresh role-successors until a cardinality lower bound is
met; role-hierarchy propagation pushes a `∀S.C` restriction along every sub-role `R ⊑ S`, not
only literal S-edges; a clash is a node asserted both a concept and its negation.

**Common pitfalls:**
- Computing `role_closure` only one hierarchy level deep — a direct `R ⊑ S` lookup misses a
  transitively implied `R ⊑ T` when `S ⊑ T` also holds; the reference implementation's
  frontier-based closure walk handles arbitrary chain length, which Task D specifically tests.
- Applying `propagate_universals` only once instead of to a fixpoint — if propagating one
  restriction creates a new successor that itself needs a restriction propagated onto *its*
  successors, a single pass can miss it; call the propagation step repeatedly until no further
  change occurs (mirroring how a real tableau algorithm iterates completion rules to a fixpoint).
- Forgetting that `check_at_least` must check for **pairwise-distinct** successors satisfying the
  concept, not just count labels loosely — in this teaching-scale fragment, treating any two
  successor nodes as automatically distinct (no merging) is an acceptable simplification, but
  students should state this simplification explicitly when writing up Task C/D, since full
  SROIQ's ≤-rule (not implemented here) is precisely about when successors must be merged instead.

**Debugging tip:** print each node's full label set and successor map after every rule
application; a silent infinite loop in Task D (fresh successors endlessly triggering further
fresh successors) is the most common failure mode and is easiest to spot by watching the trace
grow unboundedly.

**Instructor tip:** ask students to state, in one sentence, which specific SROIQ construct this
fragment leaves out (the ≤-rule and its merge step, qualified number restrictions, nominals) —
reinforces that this is a deliberately scoped teaching fragment, not a full SROIQ reasoner.

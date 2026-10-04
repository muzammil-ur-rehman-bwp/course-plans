# Lab Notes 4 — ALC Tableau Satisfiability Checker

**Concept recap:** apply ⊓/⊔/∃/∀ completion rules until either a clash (A and ¬A on one node) or
a complete clash-free branch is found; satisfiable iff some branch completes clash-free.

**Common pitfalls:**
- **Incorrect branching rule**: forgetting that ⊔ requires trying *both* disjuncts as separate
  alternatives (branching), not picking one arbitrarily — a checker that commits to one disjunct
  without backtracking will wrongly report unsatisfiable on concepts that are actually
  satisfiable under the other disjunct.
- **∀-rule applied before the matching ∃-successor exists**: the ∀-rule only adds its concept to
  an *existing* R-successor; applying it too early (before any ∃-rule has created a successor)
  silently does nothing, which can hide a clash that should have been found once the successor is
  created later — re-run the ∀-rule any time a new R-successor appears.
- **Forgetting to recurse into successor nodes**: a node's own label can be clash-free while one
  of its ∃-created successors is itself unsatisfiable once its own label is fully expanded —
  satisfiability is a property of the *whole* tree, not just the root node's label.
- In Task C, confusing which role-level a ∀ restriction applies to — ∀hasChild.∀hasChild.Doctor
  constrains grandchildren, not children, and misapplying it one level too shallow silently
  misses the intended clash or avoids one that should occur.

**Debugging tip:** reproduce both Week 4 lecture worked examples first (one clash, one open
branch) — if either disagrees with the lecture's hand-derived result, the bug is in your
completion rules or clash check, not in anything new this week.

**Instructor tip:** have students draw the tableau tree on paper (nodes, labels, role edges)
before writing code — most tableau bugs are easier to see in a hand-drawn tree than in printed
Python output.

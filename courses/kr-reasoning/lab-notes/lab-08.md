# Lab Notes 8 — AC-3 and Heuristic Backtracking Search

**Concept recap:** AC-3 removes unsupported values from each domain via a worklist of arcs,
re-queuing a variable's other arcs whenever its domain shrinks; backtracking search with MRV
picks the most-constrained variable next and LCV tries the least-disruptive value first.

**Common pitfalls:**
- Incomplete arc-consistency propagation: after `revise(domains, xi, xj, ...)` actually changes
  `domains[xi]`, forgetting to re-add `(xk, xi)` for every other neighbor `xk` of `xi` back onto
  the worklist — a value that was supported before `xi`'s domain shrank may no longer be
  supported, and skipping this re-check is the single most common AC-3 bug.
- Revising an arc in the wrong direction — `revise(domains, xi, xj, constraint)` prunes
  `domains[xi]` by checking support in `domains[xj]`, not the other way around; AC-3's worklist
  must contain both `(xi, xj)` and `(xj, xi)` as distinct arcs.
- In backtracking, checking a candidate value only against *assigned* neighbors — this is
  correct and required (checking against unassigned neighbors is what forward checking/AC-3 is
  for, not plain consistency-checking during assignment), but a common bug accidentally checks
  against *all* neighbors including unassigned ones, raising a `KeyError` on the unassigned ones.
- Recomputing MRV counts from the *original* domains instead of the domains as pruned so far
  (by AC-3 preprocessing or forward checking) — MRV must reflect the *current* domain sizes.

**Debugging tip:** after running AC-3, print each domain's size; if a domain has size 0, confirm
`ac3` returns `False` rather than continuing to backtrack on an already-empty domain.

**Instructor tip:** have students predict, before running Task C, whether AC-3 will detect the
unsatisfiability on its own — most will (incorrectly) expect it to, which makes the result a
memorable, concrete illustration of "arc consistent does not mean solvable."

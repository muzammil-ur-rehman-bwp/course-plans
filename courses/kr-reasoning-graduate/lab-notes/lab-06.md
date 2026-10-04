# Lab Notes 6 — Stable-Model Checker and Graph Coloring

**Concept recap:** M is a stable model of P iff M equals the least model of the GL-reduct P^M
(delete rules whose `not c` is contradicted by M, strip surviving `not` literals from the rest).

**Common pitfalls:**
- **Confusing negation as failure with classical negation**: `not c` means "c cannot currently
  be derived," not "c is false" — a student who encodes NAF as a classical-negation literal in
  `least_model`'s forward closure will get a monotonic (wrong) semantics.
- **Forgetting the reduct depends on the *candidate* M, not the current derived set**: the
  reduct must be recomputed fresh for every candidate M tested in `find_stable_models` — reusing
  one reduct across multiple candidates silently checks the wrong program.
- **Brute-force enumeration blowing up**: `find_stable_models` enumerates all 2^|atoms| subsets —
  fine for Task A/B's toy programs, but Task C's graph-coloring encoding needs atoms kept to the
  minimum necessary (e.g., only `assign_V_C` atoms, not also redundant helper atoms) or the
  enumeration becomes impractically slow.
- In Task C, omitting the at-least-one-color constraint and only encoding mutual exclusion —
  this silently allows a "stable model" that leaves some vertex uncolored, which is not a valid
  solution to the original problem.

**Debugging tip:** reproduce the §3 two-stable-model example first — if your checker finds a
different number of stable models there, the bug is in `gl_reduct`/`least_model`, not in your
graph-coloring encoding.

**Instructor tip:** have students compute the GL-reduct for one candidate M entirely by hand on
paper before trusting the code's output — this is the single idea (the reduct) the whole week
rests on, and it is worth slowing down for.

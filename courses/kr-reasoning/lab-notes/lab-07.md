# Lab Notes 7 — A Toy Description-Logic Subsumption Checker

**Concept recap:** `extension` computes the set of individuals satisfying a concept expression
over a fixed model; `subsumes(D, C, ...)` checks `extension(C) ⊆ extension(D)` — whether every
instance of `C` in this one model is also an instance of `D`.

**Common pitfalls:**
- Implementing `∃R.C`'s extension by checking whether *all* `R`-fillers are in `C` (that is
  `∀R.C`, not `∃R.C`) — re-read the constructor definitions carefully; `exists` only needs *one*
  qualifying filler, `forall` needs *every* filler (and is vacuously satisfied by an individual
  with no `R`-fillers at all, which students often find counterintuitive and must verify by hand).
- Treating a correct toy-checker "No" result (an axiom does not hold in *this* model) as if it
  proved the axiom false in general — this checker only tests one concrete model, not every
  possible model, so it cannot be used to prove non-subsumption in general, only to find a
  counterexample within the model actually built.
- Forgetting that a role with no entry for some individual (e.g., `bob` has no `hasChild` key at
  all) must be treated as "no fillers," not as a `KeyError` — use `roles.get(role, {}).get(x,
  set())` consistently, not direct dictionary indexing.
- Confusing DL's `⊑` ("is subsumed by" / "is a subset of") with the frame-inheritance override
  semantics from Week 6 — subsumption is a static semantic relationship checked over a model, not
  a runtime slot-resolution procedure.

**Debugging tip:** before trusting `subsumes`, print `extension(c_concept, ...)` and
`extension(d_concept, ...)` separately and manually verify the subset relationship by eye on your
small domain — this isolates whether a bug is in `extension` or in the subset check itself.

**Instructor tip:** require at least one test case per constructor (`and`, `not`, `exists`,
`forall`) in Task A — a submission that only ever tests `and` and `exists` tends to have a latent,
undetected bug in `forall`'s vacuous-truth handling.

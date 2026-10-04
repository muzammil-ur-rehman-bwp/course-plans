# Lab Notes 3 — Many-Valued and Paraconsistent Logic Evaluators

**Concept recap:** K3 and Ł3 share ∧/∨/¬ tables but differ on →; FDE represents each formula's
status as independent (true-evidence, false-evidence) pairs, so a contradiction (value B) stays
local instead of exploding into every other query.

**Common pitfalls:**
- Implementing K3's `→` as a primitive table instead of deriving it as `¬a ∨ b` — this is the
  standard construction and matches the lecture content; a hand-rolled table that disagrees with
  `¬a ∨ b` on even one entry will silently break Task B.
- In Ł3, using the wrong numeric mapping — T=1, F=0, **U=0.5** specifically; using a different
  value for U (e.g., mistakenly using 0 or 1) breaks the `min(1, 1-a+b)` formula's intended
  three-valued behavior.
- In Task D, accidentally implementing explosion anyway by letting a rule fire on "P is true"
  *and* a separate rule fire on "P is false" and then combining their consequences classically —
  the whole point of using the FDE connectives throughout is that this never happens; if a student
  needs a classical fallback to see the contrast, it must be built and run as an explicitly
  separate, clearly labeled comparison (as Task D's final sentence asks for), not mixed into the
  FDE evaluation itself.

**Debugging tip:** sanity-check `fde_and`/`fde_or` against their classical restriction first —
with every pair's evidence pair in `{(1,0), (0,1)}` only (i.e., ordinary true/false, no B or N),
confirm the FDE connectives reduce to exactly the classical truth tables before testing B/N cases.

**Instructor tip:** ask students to predict, before running Task D, what a *classical* evaluator
would output on the same KB — most will correctly predict "everything," which makes the FDE
result's restraint visibly meaningful rather than an abstract claim.

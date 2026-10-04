# Lab Notes 3 — Finite-Trace LTL Evaluator

**Concept recap:** Xφ looks one step ahead; Gφ/Fφ quantify over the (here, finite) suffix from
the current index; φUψ requires ψ eventually, with φ holding at every step strictly before that.

**Common pitfalls:**
- **Off-by-one in U**: the "until" semantics requires φ to hold at every step *strictly before*
  the step where ψ first holds (not including that step) — a common bug includes or excludes the
  wrong boundary index.
- **Out-of-bounds X at the last trace index**: `Xφ` at the final index has no next state to
  check; the reference implementation returns `False` rather than raising, but this is a
  **finite-trace simplification** — flag it explicitly rather than silently assuming it is the
  "real" infinite-trace answer.
- Evaluating Gφ/Fφ only from index 0 instead of from the queried index `i` — both operators are
  defined relative to the current position, and reusing a cached "from index 0" result at other
  indices silently gives wrong answers.
- In Task D, conflating "my evaluator has a bug" with "my evaluator correctly implements the
  stated finite-trace convention, which differs from the infinite-trace textbook semantics" —
  these are different things, and the lecture content explicitly flags the convention so it is
  not mistaken for a bug.

**Debugging tip:** reproduce the lecture's exact `G(request → F response)` example before
testing anything new — if your evaluator does not reproduce that known-correct trace result,
debug there first.

**Instructor tip:** have students manually evaluate one U-formula by hand on paper, marking the
exact index where ψ first holds and shading every index before it that must satisfy φ — this
visual check catches the off-by-one bug before it reaches code.

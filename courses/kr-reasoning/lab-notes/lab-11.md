# Lab Notes 11 — Allen's Interval Algebra and Path Consistency

**Concept recap:** `allen_relation` classifies two intervals into exactly one of 13 cases by
comparing start/end points; path consistency tightens possible relations between every interval
triple using composition, repeating to a fixed point, exactly like AC-3 for CSPs (Week 8).

**Common pitfalls:**
- Getting the `meets`/`met-by` boundary condition wrong — `meets` requires `x2 == y1` *exactly*
  (touching, no gap, no overlap); a common bug uses `<=` here, which then also (incorrectly)
  matches cases that should be `before` or `overlaps`. Check boundary conditions (`==` vs. `<`)
  against the table precisely, in the order given, since several conditions look similar.
- Checking relation conditions in an order where an earlier, broader check masks a later, more
  specific one — e.g., checking `before` (`x2 < y1`) only correctly excludes `meets` (`x2 == y1`)
  if the comparison is strict; mixing up `<` and `<=` across the 13 branches is the single most
  common source of misclassification.
- Incomplete arc/path-consistency propagation: stopping after one pass over all triples instead of
  looping until a true fixed point (no further tightening on an entire pass) — a single pass can
  miss tightenings that only become possible after an earlier triple was already tightened.
- Forgetting that path consistency with a *singleton* possible-relations set (exactly one
  relation known, not several candidates) should behave identically to the general case — do not
  special-case "known" vs. "uncertain" relations; represent every edge as a set, even if that set
  has only one element.

**Debugging tip:** when a network you believe is consistent gets tightened to an empty set, print
`possible_relations` after each full pass over all triples — the first pass where a set shrinks to
empty names exactly which triple's composition is the problem, and that triple's own composition
table entries are the first thing to re-check by hand.

**Instructor tip:** have students verify at least 2 of their `allen_relation` test cases by
literally drawing the two intervals on a timeline on paper before trusting the code — the
boundary-condition pitfall above is far easier to catch visually than symbolically.

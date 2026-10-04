# Lab Notes 11 — Black-Box Justification Finder

**Concept recap:** a justification is a minimal entailing axiom subset; black-box finding tests
candidate subsets against an unmodified entailment checker, pruning any subset that is a strict
superset of an already-found justification; an entailment can have several distinct
justifications.

**Common pitfalls:**
- Forgetting the non-minimality pruning step (`if any(j <= subset_set for j in found): continue`)
  — without it, `all_justifications` will also report every non-minimal superset of a true
  justification, flooding the result with redundant, uninformative entries.
- Designing a Task B ontology where the two "disjoint" justifications actually share an axiom —
  check this explicitly by computing the set intersection of the two intended justifications
  before running the code; a shared axiom makes Task C's "genuinely disjoint" requirement
  silently fail even if the code itself is correct.
- Testing subsets in the wrong size order — the algorithm's correctness (finding *minimal*
  justifications and correctly skipping supersets) depends on testing strictly increasing subset
  sizes; shuffling the order can report a non-minimal "justification" before its minimal subset
  has been found and recorded.

**Debugging tip:** for a small ontology (5 axioms), manually enumerate and check all 31 non-empty
subsets' entailment status in a spreadsheet or by hand before trusting the algorithm's output —
tedious but it is the only way to be fully sure the "expected" justifications used in Task C are
actually correct and complete.

**Instructor tip:** have students estimate, before running Task C on a larger ontology, how many
entailment tests `all_justifications` would perform in the worst case for 15, 20, and 25 axioms
(2^15, 2^20, 2^25) — makes the lecture's exponential-worst-case claim concrete.

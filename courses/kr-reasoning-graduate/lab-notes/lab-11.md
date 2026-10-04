# Lab Notes 11 — Rule Mining and Signal Combination

**Concept recap:** support counts confirmed rule firings; confidence is support divided by all
body firings; a grounded neuro-symbolic pattern combines mined-rule confidence with embedding
scores, each covering what the other misses.

**Common pitfalls:**
- **Variable-binding bugs in the rule miner**: `match_body`'s recursive binding check must
  enforce that a variable already bound earlier in the body (e.g., shared variable Y across
  `worksFor(X,Y)` and `locatedIn(Y,Z)`) takes the *same* value in later atoms — a miner that
  treats each atom's variables independently will overcount support/confidence.
- **Confusing support with confidence**: support alone does not tell you whether a rule is
  *reliable* — a rule firing correctly 5 times out of 5 (confidence 1.0) is stronger evidence
  than one firing correctly 5 times out of 50 (confidence 0.1), even though both have support 5.
- **Treating `combine_signals`'s ranking as authoritative**: the weighted combination in the
  lecture content is a simple, illustrative heuristic (rule confidence first, embedding score as
  tiebreak) — Task D expects students to notice and discuss its limits, not treat its output as
  ground truth.
- In Task B, forgetting that "not covered by the rule" means no *body* binding exists at all —
  not merely that the rule's confidence for that binding happens to be low.

**Debugging tip:** before trusting Task C's combined ranking, verify Task A's support/confidence
numbers by hand-counting the toy graph's matching triples — a miscount here silently invalidates
every downstream ranking.

**Instructor tip:** have students deliberately construct one "disagreement" case (as Task D
asks) rather than hoping one appears naturally — designing it forces them to understand exactly
what each signal is sensitive to.

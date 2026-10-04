# Lab Notes 12 — Empirical Complexity: Runtime Scaling

**Concept recap:** NP-completeness is a worst-case statement; random 3-SAT's empirical
difficulty peaks near a clause/variable ratio of about 4.3 (the satisfiability threshold), which
is an average-case, empirically observed phenomenon distinct from the worst-case theory.

**Common pitfalls:**
- Running too few trials per ratio (e.g., 1) and treating the resulting single-instance timing
  as representative — runtime on random instances is itself a random variable; the lab
  explicitly asks for multiple trials per ratio for exactly this reason.
- Reusing the same random seed across all ratios (or not seeding at all and getting
  inconsistent reruns) — document whatever seeding strategy is used so the sweep is at least
  internally reproducible within one run.
- Confusing "clauses" with "variables" when computing the ratio, or generating clauses with
  repeated variables (e.g., both a literal and its negation in the same clause) — `random_3sat`
  must sample 3 *distinct* variables per clause for the ratio to mean what the literature means
  by it.
- In Task D, expecting the threshold location (~4.3) to shift dramatically with problem size —
  for 3-SAT the threshold ratio is known to be relatively stable as the number of variables
  grows (it is the *sharpness* of the transition, not its location, that changes most
  noticeably); a report claiming a large threshold-location shift from 20 to 30 variables should
  be rechecked.

**Debugging tip:** before running the full sweep, sanity-check `random_3sat` by printing a few
generated clauses and confirming each has exactly 3 distinct variables and both polarities
appear across the generated formula.

**Instructor tip:** ask students to explicitly connect this lab back to Week 6's DPLL
implementation and Week 12's "why AI isn't hopeless" discussion — the spike-and-fall-off shape
they observe is the concrete evidence behind the claim that NP-completeness does not mean every
instance is equally hard.

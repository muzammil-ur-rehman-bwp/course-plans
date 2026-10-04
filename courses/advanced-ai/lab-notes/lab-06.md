# Lab Notes 6 — Equilibrium Computation: Nash vs. Correlated

**Concept recap:** zero-sum Nash equilibria are LP-solvable in polynomial time (von Neumann's
minimax theorem); general-sum Nash equilibria are PPAD-complete, with no known polynomial-time
algorithm; correlated equilibria, by contrast, are LP-solvable in polynomial time for *any*
number of players, because their defining incentive constraints are linear with no
fixed-point/parity structure.

**Common pitfalls:**
- In Task A, running too few iterations of fictitious play and mistaking slow convergence for a
  wrong implementation — fictitious play converges to the correct *average* strategy for
  zero-sum games, not necessarily smoothly round-to-round, so judge convergence by the running
  average, not the latest single-round best response.
- In Task B, checking only the indifference condition (that in-support actions tie) and
  forgetting to verify no action *outside* the assumed support is strictly better — a valid
  equilibrium support must satisfy both conditions, and skipping the second check can report a
  non-equilibrium as if it were one.
- In Task C, building the correlated-equilibrium LP's incentive constraints with the wrong sign
  (requiring a player not to *prefer* deviating, i.e., a ≤ constraint, rather than accidentally
  encoding the reverse) — double-check the constraint direction against the Week 6 lecture
  content's definition before trusting the solver's output.
- Forgetting to normalize the joint distribution (probabilities summing to 1, all non-negative)
  as an explicit LP constraint, which can let a solver return a technically feasible but
  not-actually-a-probability-distribution solution if the constraint is missing.

**Debugging tip:** validate your correlated-equilibrium LP's solution by checking it sums to 1
and is entrywise non-negative before computing welfare from it — a solver "succeeding" on a
malformed LP can silently return a meaningless vector.

**Instructor tip:** have students predict, before running Task D, whether the correlated
equilibrium's welfare will exceed the Nash equilibrium's — then connect the result back to why
real systems (e.g., coordination mechanisms with a trusted mediator or correlation device) might
deliberately use a correlated equilibrium instead of chasing a harder-to-compute Nash equilibrium.

# Lab Notes 7 — The VCG Mechanism and Its Truthfulness

**Concept recap:** VCG charges each agent exactly the externality it imposes on everyone else —
the drop in others' achievable welfare caused by that agent's presence — which is what makes
truthful reporting a dominant strategy (Week 7, §4): the term independent of the agent's own
report drops out of the maximization entirely.

**Common pitfalls:**
- Computing `best_without_i` over only the outcomes "achievable" when agent i is excluded, rather
  than over *all* outcomes in `outcomes` evaluated at the *other* agents' welfare — the payment
  rule in the Week 7 lecture content deliberately takes the max over the full outcome set, not a
  restricted one, since excluding agent i changes who contributes to welfare, not which outcomes
  exist.
- An off-by-one error in `others = [a for a in agents if a != i]` that accidentally excludes a
  second agent as well, or none at all — a quick sanity check (`len(others) == len(agents) - 1`
  for every i) catches this immediately.
- In Task C, sweeping too narrow a range around the true valuation to see the full picture — the
  utility-vs-report curve is piecewise constant with a kink exactly at the true value; the sweep
  needs to extend comfortably below and above it to show the plateau-and-kink shape, not just a
  few points near the center.
- In Task D, forgetting that with multiple items, `welfare(subset_agents, outcome)` must sum each
  agent's *bundle* valuation correctly, and that the outcome space now has combinatorially many
  bundle assignments even for 2 items and 3 bidders — a naive nested loop over all assignments is
  fine at this tiny scale but should not be assumed to stay fine as items/bidders grow.

**Debugging tip:** check the 2-bidder single-item special case by hand first — VCG's payment for
the winner should reduce to exactly the loser's valuation, independent of how the general
`vcg_mechanism` function is structured internally.

**Instructor tip:** ask students to predict the shape of the Task C utility-vs-report curve
*before* plotting it (a flat plateau at the true-value utility whenever the report still wins at
the same price, dropping to 0 once the report is low enough to lose) — this is a concrete,
visual way to make the dominant-strategy proof's abstract argument tangible.

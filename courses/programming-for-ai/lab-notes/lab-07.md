# Lab Notes 7 — CSP & Local Search

**Concept recap:** CSP = variables + domains + constraints; backtracking + forward checking
prunes inconsistent future assignments early; simulated annealing accepts worse moves
probabilistically, with acceptance probability shrinking as temperature cools.

**Common pitfalls:**
- Forward checking implemented incorrectly so it prunes too aggressively (removing values that
  are actually still consistent) — test forward checking against plain backtracking on a small
  case to verify they agree.
- Simulated annealing's temperature schedule decreasing too fast (gets stuck early, like hill
  climbing) or too slow (wastes time/never converges).
- Off-by-one in the N-Queens attacking-pairs cost function (diagonal checks are the most common
  source of bugs — remember both diagonal directions).

**Debugging tip:** for N-Queens, print the board as a grid with `Q` markers — visual inspection
catches constraint bugs much faster than reading raw column-index lists.

**Instructor tip:** ask students to predict, before running Task D, whether simulated annealing
will scale better or worse than backtracking as N grows — then have them check their prediction
against the actual runtimes.

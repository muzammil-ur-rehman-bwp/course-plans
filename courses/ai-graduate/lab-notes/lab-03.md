# Lab Notes 3 — Alpha-Beta Pruning & Nash Equilibrium

**Concept recap:** alpha-beta returns exactly the same value as minimax while skipping
provably-irrelevant branches; a Nash equilibrium is a strategy profile where no player can
unilaterally improve, which is not the same as a jointly/globally optimal outcome.

**Common pitfalls:**
- Swapping the roles of α and β, or updating the wrong bound at MIN vs. MAX nodes — this is the
  most common alpha-beta implementation bug and silently produces a value that differs from
  minimax's, defeating the whole point.
- Reporting alpha-beta's node-count reduction without a fixed move ordering — the reduction
  alpha-beta achieves depends heavily on the order children are visited in, so comparing across
  runs with different orderings is not a fair comparison.
- **Confusing a Nash equilibrium with a globally optimal outcome** — a student who finds
  (Defect, Defect) as the unique equilibrium in the Prisoner's Dilemma but concludes "so this is
  the best outcome for both players" has missed the entire point of Task D; a Nash equilibrium
  is about unilateral deviation, not joint welfare.
- Off-by-one in indexing the payoff matrix when checking `row_optimal`/`col_optimal` — double
  check which axis is the row player's choice vs. the column player's choice.

**Debugging tip:** for Task B, verify alpha-beta's *returned value* matches Task A's minimax
value exactly on the same game tree before reporting any node-count reduction — a wrong value
with fewer nodes visited is not a successful optimization, it is a bug.

**Instructor tip:** explicitly ask students, during Task D, to state in one sentence why
(Cooperate, Cooperate) is not a Nash equilibrium even though it is better for both players — this
directly targets the common misconception this lab is designed to correct.

# Lab Notes 5 — Independent Q-Learners in Repeated Matrix Games

**Concept recap:** independent Q-learning applies a correct single-agent (bandit-style) update
to each player separately, but each player's effective reward depends on the other player's
current, still-changing policy — this breaks the fixed-target assumption single-agent
convergence proofs rely on, and can produce indefinite cycling rather than convergence.

**Common pitfalls:**
- Using too high an `epsilon` or too few episodes to let cycling become visible in Task B — the
  matching-pennies cycle is a statistical pattern in action *frequencies* over a window, not a
  single deterministic back-and-forth; 20,000 episodes is close to the minimum needed to see a
  clean picture with this simple a learner.
- Entering a payoff matrix with rows and columns swapped relative to which player it represents —
  verify by hand, for at least one action profile, that `payoff_a[a][b]` and `payoff_b[a][b]`
  give the payoffs you intend before trusting a 20,000-episode run built on a mis-entered matrix.
- In Task C, picking a "coordination game" that is actually a game with multiple equally-good
  pure equilibria (not a unique one that is also the social optimum) — this can make convergence
  look flaky for reasons unrelated to non-stationarity; use a payoff structure where one
  equilibrium strictly dominates the others for both players.
- In Task D, forgetting that a joint-action learner needs the *other* agent's current action
  each round to index into Q(a, b) — if both players are joint-action learners simultaneously,
  each conditions on the other's immediately-preceding action, not a magically-known current one.

**Debugging tip:** before the full 20,000-episode runs, verify both `Qa` and `Qb` update
correctly by hand for 2–3 manually traced rounds of a simple deterministic payoff matrix.

**Instructor tip:** ask students to predict, before running Tasks B and C, which game will show
visible cycling and which will stabilize — then ask them to predict whether the Task D
joint-action learner will still cycle on matching pennies (it generally reduces but does not
eliminate the fundamental issue, since matching pennies has no pure equilibrium for *any* learner
to converge to), which is a useful, concrete illustration that modeling the other agent is not a
universal fix for non-stationarity.

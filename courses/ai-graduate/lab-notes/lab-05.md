# Lab Notes 5 — Rigorous CSP & Simulated Annealing

**Concept recap:** AC-3 enforces arc consistency as a pruning step (necessary, not sufficient,
for a solution); simulated annealing accepts worsening moves with probability exp(−Δ/T), trading
exploration (high T) for exploitation (low T) as the schedule cools.

**Common pitfalls:**
- Treating AC-3 reaching a fixed point as "solved" — AC-3 only prunes locally inconsistent
  values; a CSP can be arc-consistent and still have no global solution (or need further search
  to find one), which Task A/B's pairing is designed to illustrate.
- In Task C, computing the Metropolis acceptance probability as exp(+Δ/T) instead of exp(−Δ/T)
  (sign error) — this inverts the algorithm's behavior, making it reject good moves and accept
  bad ones more often as it should be doing the opposite.
- Letting the temperature reach exactly 0 and then dividing by it in the acceptance-probability
  formula — guard against T ≤ some small epsilon before computing `exp(-delta/T)`.
- In Task D, concluding "faster cooling is always worse" from a single run — simulated annealing
  is stochastic; a single comparison can be misleading, which previews Week 13's statistical-
  significance lesson (multiple seeds matter even for an informal lab comparison).

**Debugging tip:** for Task C, plot the *temperature* schedule itself alongside tour cost — a
schedule that cools too fast will show tour cost "freezing" early at a poor value; too slow and
it barely improves within the iteration budget.

**Instructor tip:** ask students to connect the Metropolis criterion explicitly to Week 9's
exploration-exploitation tradeoff in reinforcement learning — the same high-level
exploration/exploitation idea appears in both, just embedded differently.

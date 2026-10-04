# Lab Notes 9 — Policy Iteration & Q-Learning

**Concept recap:** policy iteration alternates exact policy evaluation and greedy improvement,
converging in finitely many steps; Q-learning learns Q(s,a) from sampled transitions with the
update Q(s,a) ← Q(s,a) + α[r + γ·max_a' Q(s',a') − Q(s,a)], without needing a known model.

**Common pitfalls:**
- In policy evaluation, iterating only a fixed small number of sweeps instead of to convergence
  (θ threshold) — this makes "policy iteration" silently behave like a hybrid of value and
  policy iteration, which can still work but no longer matches the Week 9 finite-convergence
  argument exactly as stated.
- **Q-learning update bugs**: using `Q[s][a]` instead of `max(Q[s2].values())` for the
  bootstrap target (bootstrapping from the wrong state), or forgetting to treat a terminal
  transition's bootstrap term as 0 — both silently corrupt the learned values.
- Confusing Q-learning's off-policy update (always uses max_a' Q(s',a') regardless of which
  action was actually taken next) with an on-policy update (would use the action the ε-greedy
  policy actually selected next) — these are different algorithms (Q-learning vs. SARSA); this
  lab specifically implements Q-learning.
- In Task C, expecting ε = 0 to simply "learn slower" rather than potentially getting
  permanently stuck on a poor initial action — with an all-zero Q-table and no exploration, ties
  are broken arbitrarily and the agent may never try the action sequence leading to the +1 goal
  at all within the episode budget.

**Debugging tip:** compare Task A and Task B's policies state-by-state and print any states
where they disagree — early in training, Q-learning's policy will legitimately differ in rarely-
visited states; persistent disagreement in commonly-visited states signals a bug.

**Instructor tip:** ask students to explain, specifically, why policy iteration's convergence
guarantee (finite iterations) does not transfer to Q-learning — Q-learning's "convergence" is a
different (asymptotic, stochastic-approximation) guarantee requiring sufficient exploration and
an appropriately decaying learning rate, not a finite-steps guarantee.

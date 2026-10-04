# Lab Notes 4 — Contextual Bandits vs. a Context-Free Baseline

**Concept recap:** a contextual bandit's confidence bonus (α·√(xᵀA⁻¹x)) generalizes UCB1's
confidence radius to a context-dependent linear reward model; the bonus shrinks as more, and
more diverse, contexts have been observed for an arm, exactly mirroring UCB1's
more-pulls-shrink-the-bound behavior from Week 3.

**Common pitfalls:**
- Forgetting the ridge term (`reg * np.eye(d)`) when initializing `A`, which makes the initial
  `A` singular and `np.linalg.inv(self.A)` fail on the very first call — the ridge term is not
  optional bookkeeping, it is what keeps the regression well-posed before any data arrives.
- Drawing the context only once and reusing it across rounds — the contextual-bandit protocol
  requires a **fresh** context each round (Week 4, §1); reusing one context silently turns the
  exercise back into a context-free bandit and will make the context-free baseline look
  artificially competitive.
- In Task C, running too few seeds or too short a horizon to see LinUCB's advantage emerge — with
  only d=4 features, the ridge estimate needs enough rounds per arm to converge; T=3000 is a
  deliberate minimum, not a suggestion.
- In Task D, confusing a too-large `alpha` (over-exploration, high early regret that never fully
  recovers within T=3000) with a bug in the implementation itself — verify correctness at a
  moderate `alpha` first before using the sweep to diagnose the exploration-exploitation tradeoff.

**Debugging tip:** before trusting the full comparison, verify `LinUCBArm.score` on a trivial
case where all arms share the same θₐ — both algorithms' average regret should converge toward
zero, since context carries no information about which arm is better in that case.

**Instructor tip:** ask students to predict, before running Task C, whether the context-free
baseline's regret should grow linearly or sublinearly in T here — since the context-averaged
reward is still fixed per arm, context-free UCB1 still achieves O(log T) regret against the
*context-averaged* optimum, but that is a much weaker guarantee than LinUCB's regret against the
*context-dependent* optimal policy, and the gap between the two curves is exactly this
distinction made visible.

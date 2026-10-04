# Lab Notes 10 — POMDP Belief-State Updates

**Concept recap:** the belief update is a two-step Bayesian filter — predict forward with the
transition model, then reweight by the observation model and normalize so beliefs sum to 1.

**Common pitfalls:**
- **Forgetting to normalize** after the reweighting step — `belief_update`'s `unnormalized`
  dict must be divided by its total; skipping this produces values that do not sum to 1 and are
  not valid probabilities.
- Applying the observation model to the *prior* belief instead of the *predicted* belief
  (skipping the prediction/transition step entirely) — this silently reduces the POMDP belief
  update to a plain one-shot Bayes'-rule calculation and ignores the action's effect on the
  state, which matters whenever the transition model is not the identity (unlike the "stay"
  action used in the simplest worked example).
- Raising a division-by-zero error (or silently producing NaNs) when an observation has zero
  probability under the predicted belief — this is a real edge case (an "impossible" observation
  given the model) that should be handled explicitly, as the lecture's `belief_update` does with
  a raised `ValueError`, not silently ignored.
- In Task D, expecting belief to reach exactly 1.0 after enough observations — with a
  nondegenerate (nonzero false-positive/negative) observation model, belief asymptotically
  approaches but mathematically never reaches 1 or 0; reporting "it reached 1.0" is a sign of a
  rounding-display issue or a conceptual misunderstanding, not a true belief of certainty.

**Debugging tip:** after every belief update, assert that the resulting dictionary's values sum
to 1.0 within floating-point tolerance (e.g., `abs(sum(belief.values()) - 1.0) < 1e-9`) as a
built-in sanity check in your own code, not just a manual visual check.

**Instructor tip:** ask students to state, in one sentence, why a belief state is called a
"sufficient statistic" for the agent's history — this is the structural reason POMDP planning
can be framed over belief states at all, rather than needing to remember the entire history.

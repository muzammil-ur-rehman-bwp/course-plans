# Week 12 Summary — Probabilistic Reasoning and Bayesian Networks in Depth

**Key takeaways:**
- Variable elimination answers a Bayesian-network query by eliminating hidden variables one at a
  time — multiplying the factors that mention a variable, then summing it out — rather than
  forming the full joint distribution as enumeration does.
- A factor is a function from variable assignments to a number; `multiply` combines two factors
  over their shared and distinct variables, `sum_out` marginalizes a variable away.
- Elimination order affects efficiency, not correctness; any valid order produces the same exact
  answer.

**You should now be able to:** implement a `Factor` class with multiply and sum-out operations;
implement and trace variable elimination on a small network; explain why it avoids the full-joint
cost that enumeration incurs.

**Next week:** reasoning with uncertainty beyond Bayes — Markov logic networks and fuzzy logic.

# Week 3 Summary — Multi-Armed Bandits

**Key takeaways:**
- The bandit setting restricts feedback to only the reward of the chosen arm, formalizing the
  exploration-exploitation tradeoff.
- UCB1's confidence radius comes directly from the Hoeffding bound; its regret bound,
  E[Regret_T] = O((K log T)/Δ_min), is derived from a bound on each suboptimal arm's expected
  pull count.
- Thompson sampling achieves a similar explore/exploit balance via posterior sampling rather
  than an explicit confidence bound.

**You should now be able to:** state and apply the Hoeffding bound; derive UCB1's regret bound;
implement UCB1 and compare it empirically against ε-greedy.

**Next week:** Contextual bandits — the bridge between bandits and full RL, and where tabular
Q-learning sits on that spectrum.

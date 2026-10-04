# Week 2 Summary — Online Learning and Regret Minimization

**Key takeaways:**
- The online-learning protocol makes no statistical assumption on losses; regret (vs. the best
  fixed expert in hindsight) is the right performance measure.
- The multiplicative-weights algorithm's regret bound, Regret_T ≤ ηT + (ln N)/η, is derived via a
  potential-function argument; optimizing η gives Regret_T = O(√(T ln N)).
- Regret grows sublinearly in T and only logarithmically in the number of experts N.

**You should now be able to:** state the online-learning protocol and regret precisely; derive
the multiplicative-weights regret bound; implement it and verify sublinear regret empirically.

**Next week:** Multi-armed bandits — the UCB1 algorithm and its regret bound, derived from a
Hoeffding concentration argument, plus Thompson sampling conceptually.

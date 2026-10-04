# Week 9 Summary — Midterm Exam; Markov Decision Processes II

**Key takeaways:**
- Policy iteration alternates exact policy evaluation and greedy policy improvement, and
  provably converges to the optimal policy in finitely many iterations for a finite MDP (unlike
  value iteration's asymptotic convergence).
- The exploration-exploitation tradeoff is fundamental to reinforcement learning; ε-greedy is the
  standard simple way to balance exploring new actions against exploiting the current estimate.
- Q-learning learns Q(s,a) directly from sampled transitions with the update rule
  Q(s,a) ← Q(s,a) + α[r + γ·max_a' Q(s',a') − Q(s,a)], and is off-policy and model-free.

**You should now be able to:** implement policy iteration and confirm it converges to the same
optimal policy as value iteration; implement tabular Q-learning with ε-greedy exploration;
explain the difference between model-based (value/policy iteration) and model-free (Q-learning)
approaches.

**Next week:** Partially Observable MDPs (POMDPs) — belief states and why partial observability
complicates planning.

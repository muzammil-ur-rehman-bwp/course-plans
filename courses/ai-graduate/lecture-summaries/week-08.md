# Week 8 Summary — Markov Decision Processes I

**Key takeaways:**
- An MDP is ⟨S, A, P, R, γ⟩; the Bellman optimality equation defines V*(s) recursively as the
  best immediate expected reward plus discounted future optimal value.
- Value iteration repeatedly applies the Bellman backup to every state until the values stop
  changing by more than a threshold ε.
- Value iteration converges because the Bellman optimality operator is a contraction mapping
  with factor γ under the max-norm; this requires γ < 1, and setting γ = 1 (or higher) can break
  the convergence guarantee.
- Midterm (Week 9) covers Weeks 1–8: advanced search, game theory/multi-agent systems, rigorous
  CSP/metaheuristics, SAT/SMT, rigorous planning, and this week's MDP formalism.

**You should now be able to:** state the Bellman optimality equation; implement value iteration
and extract a greedy optimal policy from the resulting value function; explain why γ < 1 is
necessary for the convergence guarantee.

**Next week:** Midterm Exam, followed by MDPs II — policy iteration, the exploration-exploitation
tradeoff, and Q-learning.

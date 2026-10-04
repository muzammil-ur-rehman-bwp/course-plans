# Week 5 Summary — Rigorous CSP & Combinatorial Optimization

**Key takeaways:**
- AC-3 enforces arc consistency (O(cd³) worst case) as a pruning step before/during backtracking
  search; MRV and LCV ordering heuristics substantially reduce the practical search tree.
- Simulated annealing's Metropolis acceptance criterion accepts worsening moves with probability
  exp(−Δ/T), literally embedding exploration (high T) vs. exploitation (low T).
- Simulated annealing has a real but impractical asymptotic convergence guarantee under a
  logarithmic cooling schedule; practical geometric schedules sacrifice that guarantee for speed.
- Genetic algorithms (selection, crossover, mutation) have no general convergence guarantee and
  can suffer premature convergence to a local optimum.

**You should now be able to:** implement AC-3 and heuristic backtracking for a CSP; implement
simulated annealing and a genetic algorithm for a toy optimization problem; explain the
convergence-guarantee gap between the two metaheuristics.

**Next week:** Automated reasoning — the DPLL algorithm for SAT in depth, and a conceptual
overview of SMT.

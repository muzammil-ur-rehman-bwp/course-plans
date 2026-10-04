# Week 6 Summary — Automated Reasoning: SAT and SMT

**Key takeaways:**
- DPLL is a sound, complete backtracking search for SAT built on unit propagation and
  pure-literal elimination before branching on an unassigned variable.
- Modern CDCL solvers add clause learning: analyzing a conflict to derive a new clause and
  backtracking non-chronologically, which is why industrial SAT solvers scale far beyond plain
  DPLL.
- SMT generalizes SAT by combining a SAT solver's Boolean reasoning with theory-specific decision
  procedures (e.g., linear arithmetic), enabling verification and scheduling applications SAT
  alone cannot naturally express.

**You should now be able to:** implement DPLL with unit propagation; trace DPLL by hand on a
small CNF formula; explain conceptually how clause learning and SMT extend basic SAT solving.

**Next week:** Rigorous classical planning — the PSPACE-completeness of planning,
relaxed-planning-graph heuristics, and Hierarchical Task Network (HTN) planning.

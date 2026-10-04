# Week 5 Summary — Rule-Based Systems

**Key takeaways:**
- A production system has working memory, a rule base, and a recognize-act inference cycle;
  conflict resolution picks among multiple applicable rules.
- Forward chaining is data-driven (derive everything reachable); backward chaining is
  goal-directed (derive only what a specific query needs); both are sound and complete over the
  same rule base.
- A well-designed rule engine is decoupled from any single domain — the same `forward_chain`/
  `backward_chain` code runs unchanged over unrelated rule bases.

**You should now be able to:** describe the production-system architecture; implement
domain-independent forward- and backward-chaining functions; apply the same engine to two
different toy rule bases without modifying the engine.

**Next week:** semantic networks and frames — structured taxonomic knowledge, inheritance, and
the non-monotonic inheritance (exceptions) problem.

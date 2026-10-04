# Week 9 Summary — Midterm Exam; Formal Verification for Knowledge-Based Systems

**Key takeaways:**
- Model checking decides `M ⊨ φ`: whether every infinite path of a finite Kripke structure M
  starting from an initial state satisfies LTL formula φ.
- The automata-theoretic approach: translate ¬φ to a Büchi automaton, form the product with M,
  and check for a reachable accepting cycle; M ⊨ φ iff none exists.
- LTL model checking is PSPACE-complete in formula size (searched on-the-fly, avoiding the
  automaton's potential exponential blow-up) and polynomial in model size; state-space size is
  the real industrial bottleneck.

**You should now be able to:** state the model-checking problem precisely; implement a small
explicit-state LTL checker for safety/liveness properties via cycle detection.

**Next week:** Multi-agent belief merging — extending AGM revision to multiple peer agents.

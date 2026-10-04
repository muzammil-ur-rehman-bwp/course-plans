# Week 12 Summary — Computational Complexity of AI Problems

**Key takeaways:**
- SAT and general CSP are NP-complete (Cook-Levin theorem for SAT; 3-SAT reduction for CSP);
  classical planning is PSPACE-complete (Week 7); NP ⊆ PSPACE, with strict containment widely
  believed but, like P vs. NP, unproven.
- These are worst-case results — they do not imply every instance is hard, which is why
  heuristics, approximation, and structure exploitation let practical AI systems cope with
  problems that are intractable in the worst case.
- Random 3-SAT's empirical difficulty spikes near a clause/variable ratio of about 4.3 (the
  satisfiability threshold), a concrete illustration of average-case vs. worst-case difficulty.

**You should now be able to:** state precisely what NP-complete and PSPACE-complete mean;
classify SAT, CSP, and planning correctly by complexity class; run and interpret an empirical
runtime-scaling experiment.

**Next week:** Research methods in AI — how to read and critique a paper, reproducibility, and
sound experimental design.

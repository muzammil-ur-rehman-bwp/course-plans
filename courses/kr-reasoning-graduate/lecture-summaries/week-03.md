# Week 3 Summary — Temporal Logic

**Key takeaways:**
- LTL evaluates X (next), G (always), F (eventually), and U (until) over a single execution
  trace; this course uses an explicitly flagged finite-trace convention for labs.
- CTL adds path quantifiers A/E to reason over a branching computation tree; AGφ/EFφ are
  properties of the whole branching structure and are not interchangeable with LTL's Gφ/Fφ on one
  trace.
- LTL/CTL let planning goals and safety/liveness properties (Fgoal, G¬fail,
  G(request→Fresponse)) be stated precisely, extending the undergraduate course's single-goal-test
  STRIPS view.

**You should now be able to:** evaluate an LTL formula against a finite trace by hand and in code;
translate an English planning/safety/liveness requirement into LTL; explain why CTL's AG/EF are
not the same as LTL's G/F once branching is allowed.

**Next week:** Description logics in depth — the ALC tableau algorithm for concept
satisfiability, DL reasoning complexity (ALC is PSPACE-complete), and the OWL 2 EL/QL/RL
profiles.

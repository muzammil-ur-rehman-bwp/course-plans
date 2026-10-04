# Week 4 Summary — Description Logics in Depth

**Key takeaways:**
- The ALC tableau algorithm decides concept satisfiability by trying to build a model via
  completion rules (⊓, ⊔, ∃, ∀), with a clash (A and ¬A on one node) meaning that branch fails;
  satisfiable iff some branch completes clash-free.
- ALC concept satisfiability is PSPACE-complete; more expressive DLs (e.g., SHIQ) push reasoning
  to EXPTIME-complete.
- OWL 2 EL/QL/RL each trade away expressiveness for a specific tractability guarantee
  (polynomial subsumption, DB-rewritable queries, and rule-engine-implementable reasoning,
  respectively).

**You should now be able to:** run the ALC tableau algorithm by hand and in code on a small
concept; state the PSPACE-completeness result for ALC precisely; match an application's
expressiveness/tractability needs to the right OWL 2 profile.

**Next week:** Automated theorem proving in depth — the sequent calculus, first-order tableau
(extending this week's DL tableau to full FOL), and resolution refinement strategies (set-of-
support, ordering) beyond the undergraduate course's basic resolution.

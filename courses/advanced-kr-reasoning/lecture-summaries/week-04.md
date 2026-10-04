# Week 4 Summary — Advanced Description Logics: SROIQ

**Key takeaways:**
- SROIQ extends ALC with role hierarchies, complex role inclusion axioms (under a regularity
  condition), qualified number restrictions, nominals, and role characteristics.
- The tableau gains a ≥-rule (create successors) and a ≤-rule (merge successors), plus
  role-hierarchy propagation of ∀-restrictions along sub-roles.
- SROIQ concept satisfiability is N2ExpTime-complete — a real jump beyond ALC's PSPACE and
  SHIQ's EXPTIME, driven by nominals interacting with inverse roles/number restrictions; still
  decidable only given the regularity condition.

**You should now be able to:** enumerate SROIQ's new constructs; trace the ≥/≤-rule extensions by
hand; state the N2ExpTime-completeness result and what drives it.

**Next week:** Structured argumentation — the ASPIC+ framework, and rebutting vs. undercutting
attacks. **Assignment 1 assigned this week.**

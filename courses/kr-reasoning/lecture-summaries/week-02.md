# Week 2 Summary — Propositional Logic in Depth

**Key takeaways:**
- CNF conversion follows a fixed four-step procedure (eliminate `↔`, eliminate `→`, push
  negation inward, distribute `∨` over `∧`); DNF is the dual, distributing `∧` over `∨`.
- The resolution rule combines two clauses on a complementary literal pair; resolution refutation
  proves entailment by negating the query and deriving the empty clause.
- SAT is NP-complete: no known polynomial algorithm exists, which is why model checking and
  resolution-based reasoning both carry a worst-case exponential cost that must be stated
  honestly, not hidden.

**You should now be able to:** convert any propositional sentence to CNF or DNF; implement and
trace resolution refutation on a small clause set; explain what NP-completeness implies for the
scalability of propositional reasoning.

**Next week:** first-order logic in depth — syntax, semantics, and translating English to FOL.

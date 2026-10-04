# Week 7 Summary — Propositional Logic Inference

**Key takeaways:**
- `KB ⊨ α` (entailment) means `α` is true in every model where `KB` is true; model checking
  verifies this by brute-force enumeration.
- Resolution operates on clauses in CNF; resolution refutation proves entailment by deriving the
  empty clause after negating the query.
- Horn-clause knowledge bases support efficient forward chaining (data-driven) and backward
  chaining (goal-driven), both of which are sound and complete for definite clauses.

**You should now be able to:** convert a sentence to CNF; implement resolution refutation for a
small knowledge base; trace forward and backward chaining by hand on a Horn-clause KB.

**Next week:** first-order logic — representing objects, relations, and quantifiers, plus
midterm review.

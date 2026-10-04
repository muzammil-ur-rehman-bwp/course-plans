# Week 3 Summary — First-Order Logic in Depth

**Key takeaways:**
- FOL adds terms (constants, variables, functions), predicates, and quantifiers (`∀`, `∃`) to
  close the expressiveness gap propositional logic cannot close.
- A model consists of a domain plus an interpretation of every constant/predicate/function; a
  quantified sentence's truth is defined recursively over assignments of domain objects to
  variables.
- Quantifier order is not interchangeable: `∀x∃y φ` is strictly weaker than `∃y∀x φ`; "all P are
  Q" translates with implication (`→`), not conjunction (`∧`).

**You should now be able to:** parse and construct FOL sentences with nested quantifiers;
evaluate a quantified sentence against a small finite-domain model by brute force; translate
English sentences to FOL correctly, avoiding the implication/conjunction and scope-order traps.

**Next week:** first-order inference — unification, FOL resolution, and Skolemization.

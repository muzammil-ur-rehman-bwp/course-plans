# Week 4 Summary — First-Order Inference

**Key takeaways:**
- Unification finds the most general substitution making two expressions identical; the occurs
  check prevents unifying a variable with a term that contains it (which would build an infinite
  term).
- FOL resolution unifies a complementary literal pair before resolving and applies the resulting
  substitution to the whole resolvent; variables must be renamed apart across clauses first to
  avoid variable capture.
- Skolemization removes existential quantifiers by replacing them with new function/constant
  symbols before resolution; FOL resolution is sound and refutation-complete, but not guaranteed
  to terminate on satisfiable sentence sets.

**You should now be able to:** implement unification with an occurs check; implement a FOL
resolution step that unifies before resolving; Skolemize a simple `∃`-containing sentence by hand;
state what soundness and refutation-completeness mean for FOL resolution.

**Next week:** rule-based systems — production systems, forward/backward chaining, and building a
general-purpose Python rule engine.

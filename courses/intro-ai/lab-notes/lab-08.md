# Lab Notes 8 — First-Order Logic: Translation & Well-Formedness

**Concept recap:** FOL adds objects, predicates, functions, and quantifiers (∀, ∃) to
propositional logic; translating English requires identifying objects/predicates and getting
quantifier order right, since swapping `∀x ∃y` and `∃y ∀x` changes the meaning.

**Common pitfalls:**
- Translating "All P are Q" as `∀x P(x) ∧ Q(x)` instead of the correct `∀x P(x) → Q(x)` — the
  first claims *everything* is both a P and a Q, which is almost always wrong.
- Translating "Some P are Q" as `∃x P(x) → Q(x)` instead of `∃x P(x) ∧ Q(x)` — an implication
  inside an existential is vacuously satisfied by any object that is not a P at all.
- Getting quantifier scope backwards on sentences like "Every student has a favorite course" —
  always ask "does this mean one shared y for everyone, or does each x get its own y?"

**Debugging tip:** for any FOL translation that "feels wrong," substitute a tiny concrete domain
(e.g., 2 objects) and check whether the formula's truth value under a hand-built model matches
your intuitive reading of the English sentence.

**Instructor tip:** the ∀∃ vs. ∃∀ translations above are the single most common point of
confusion in this unit — budget extra board-work time for 2–3 contrasting pairs before moving to
the lab's checker code, which only validates syntax, not meaning.

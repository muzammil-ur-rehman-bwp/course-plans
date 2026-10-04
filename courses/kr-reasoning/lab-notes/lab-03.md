# Lab Notes 3 — Finite-Domain FOL Models and Translation

**Concept recap:** a finite-domain model evaluates quantified sentences by enumeration —
`∀x φ(x)` checks `φ` for every domain object, `∃x φ(x)` checks whether at least one satisfies it;
nested quantifiers compose these checks, and order changes meaning.

**Common pitfalls:**
- Translating "all P are Q" as `∀x (P(x) ∧ Q(x))` instead of `∀x (P(x) → Q(x))` — the conjunction
  version incorrectly asserts every domain object is both P and Q, not just that P implies Q.
- Swapping `∀x∃y` and `∃y∀x` carelessly when translating "everyone likes someone" (`∀x∃y`) versus
  "someone is liked by everyone" (`∃y∀x`) — these are genuinely different claims, not stylistic
  variants.
- Building a Task C demonstration relation that accidentally makes both forms true or both false
  — deliberately construct a relation where each `x` is related to a *different* `y` to force the
  asymmetry to show up.
- Off-by-one domain errors: forgetting that a model's domain must be non-empty, or including
  objects in the domain that never appear in any relation tuple, silently making universal claims
  vacuously true or false in unintended ways.

**Debugging tip:** for any quantified sentence, print the full table of which domain-object
assignments satisfy the inner predicate before trusting the `forall`/`exists` result — this makes
scope-order bugs visible immediately.

**Instructor tip:** require students to state, in English, which of the two scope-trap sentences
is the stronger claim *before* they write any code — students who cannot do this on paper
invariably build a model that "confirms" whichever bug they already have.

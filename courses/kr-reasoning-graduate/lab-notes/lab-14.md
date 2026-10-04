# Lab Notes 14 — Proof-Tree and "Why Not" Explanation Generator

**Concept recap:** a proof tree records, recursively, the rule and premises that justified each
derived fact down to given leaf facts; a "why not" explanation recurses through a failing
query's rule chain to the actual missing leaf fact(s).

**Common pitfalls:**
- **Proof tree pointing to stale nodes**: `forward_chain_with_proof` must build each new fact's
  `ProofNode` from the *current* `proof_of` entries of its premises at the moment it fires — if a
  premise's proof is later replaced (shouldn't happen in a monotonic forward-chaining engine, but
  worth checking), a stale reference would silently misrepresent the derivation.
- **Why-not reporting the first-level gap only**: Task B's single-level `why_not` correctly
  reports "CanFlap(polly) is missing," but that is not yet the full explanation a user wants —
  Task C's recursion is the actual lab objective; stopping at Task B's output is an incomplete
  submission.
- **Infinite recursion on a self-referential rule base**: a rule base with a cycle (rule A's
  premise depends, through other rules, on rule A's own conclusion) can make a naive recursive
  `why_not` loop forever — guard with a visited-set if constructing Task D's larger rule base.
- Confusing "no rule has this as a conclusion" (query is simply not derivable by any rule in the
  knowledge base) with "a rule exists but a premise is missing" — `why_not` must distinguish
  these two cases, not report `None` for both.

**Debugging tip:** reproduce the exact Flies(tweety)/Flies(polly) worked examples from the
lecture content before testing Task D's larger rule base — these are the two known-correct fixed
points.

**Instructor tip:** have students compare a proof tree's "why" output against a why-not trace's
output side by side on closely related queries (Flies(tweety) succeeds, Flies(polly) fails) —
seeing both explanation types for structurally similar queries makes the contrast concrete.

# Lab Notes 8 — Grounded and Preferred Extension Calculator

**Concept recap:** the grounded extension is the least fixed point of F(S) = {a : S defends a},
found by iterating from ∅; a preferred extension is a maximal admissible (conflict-free,
self-defending) set; the two can diverge (grounded empty, several preferred extensions).

**Common pitfalls:**
- **Computing the wrong argumentation extension for the question asked**: grounded and
  preferred answer different questions (the unique cautious answer vs. the maximal committed
  alternatives) — reporting a preferred extension when a grounded extension was asked for (or
  vice versa) is a correctness bug, not a style choice.
- **Not iterating F to a true fixed point**: a single application of `characteristic_function`
  is not the grounded extension — `grounded_extension` must keep iterating until `new_S == S`;
  stopping after one or two iterations on a longer chain (as in Task D) silently under-derives.
- **Admissibility check forgetting conflict-freeness**: a set that defends all its members but
  contains an internal attack is not admissible — `is_admissible` must check both conditions, not
  just the defense condition.
- In Task C, forgetting that the *order* attacks are listed in does not matter but the attacker
  map must be rebuilt (not reused from Task B) once a new argument (d) is added — a stale
  attacker map silently ignores d's attack on a.

**Debugging tip:** reproduce both the §3 chain example and the §4 mutual-attack example exactly
first — these are the two hand-derived fixed points everything else should be checked against.

**Instructor tip:** have students physically draw the attack graph with arrows before computing
anything — most grounded/preferred extension bugs trace back to a misread attack direction
(a→b vs. b→a) rather than a logic error in the fixpoint code itself.

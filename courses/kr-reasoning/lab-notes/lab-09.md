# Lab Notes 9 — Closed-World Queries and Default-Logic Extensions

**Concept recap:** CWA treats anything not derived as false; a default fires only while its
justification is not contradicted by a currently-derived fact — adding a fact that contradicts a
justification can remove a conclusion that was previously derivable, which is the entire point of
this week's labs.

**Common pitfalls:**
- Confusing default/non-monotonic inheritance (Week 6's frame override, a structural
  representation choice) with default *logic* (this week's explicit prerequisite/justification/
  consequent rule, a reasoning formalism) — Task C explicitly asks why Week 5's strict-rule engine
  cannot replicate this, and the answer is about strict rules never removing conclusions, not
  about frames versus rules as representations.
- Implementing the "blocked" check in `apply_defaults` by testing whether the justification is
  simply *absent* from `derived`, rather than whether its negation is *present* — under this
  course's simple negation-as-a-fact convention (`"not_X"`), a justification being merely unknown
  should **not** block a default; only an explicit `"not_X"` fact should. Conflating these two
  checks causes a default to fire in Task B but then incorrectly never fire at all once any
  unrelated fact is added.
- Forgetting the CWA engine must *not* simply return `atom in facts` — it must run the rules to a
  fixed point first (exactly like Week 5's forward chaining) and only then decide the atom is
  false, or it will under-derive true facts.
- Running `apply_defaults` only once instead of to a fixed point — a default's consequent might
  itself be another default's prerequisite; stop only when a full pass derives nothing new.

**Debugging tip:** for a retraction case that isn't retracting, print the `blocked` boolean for
the specific default on each pass — if it stays `False` after adding the blocking fact, the
`"not_" + justification` check is either misspelled or checking the wrong fact name.

**Instructor tip:** have students write out, in English, exactly which single boolean value in
`apply_defaults` flips between Task B (fires) and Task C (blocked) — if they cannot name it, they
have not yet located the mechanism that makes the reasoning non-monotonic.

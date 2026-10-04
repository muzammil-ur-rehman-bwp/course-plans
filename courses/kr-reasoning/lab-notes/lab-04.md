# Lab Notes 4 — Unification and First-Order Resolution

**Concept recap:** unification finds the most general substitution making two expressions
identical, failing on a mismatched function symbol/arity or an occurs-check violation; FOL
resolution renames variables apart, unifies a complementary literal pair, and applies the
resulting substitution to the whole resolvent.

**Common pitfalls:**
- Forgetting to rename variables apart (`standardize_apart`) before resolving two clauses that
  both use `x` — this causes **variable capture**: the solver silently unifies two variables that
  were never meant to be related, producing a resolvent that looks plausible but is not a valid
  logical consequence.
- Omitting the occurs check "to keep things simple" — this lets `unify(x, f(x))` succeed and
  produce a substitution that, if ever applied/expanded, recurses forever; always implement and
  test the occurs check explicitly, not just the happy path.
- Applying a substitution only to the *resolved* literals and not to the rest of the resolvent
  clause — every literal carried over from either parent clause must have the MGU applied to it.
- Confusing a failed unification (`None`) with an *empty* substitution (`{}`) — these mean
  opposite things ("no unifier exists" vs. "the two expressions already matched with no bindings
  needed") and must be checked with `is None`, never with plain truthiness.

**Debugging tip:** print the substitution dictionary after every recursive call inside `unify`;
a substitution that grows to include a variable mapped to a term containing itself is the
signature of a missing or broken occurs check.

**Instructor tip:** have students manually trace the unification of `Likes(x, mother(x))` with
`Likes(john, mother(john))` on paper, step by step, before coding — this is the one worked
example from lecture, so it doubles as an independent correctness check for their implementation.

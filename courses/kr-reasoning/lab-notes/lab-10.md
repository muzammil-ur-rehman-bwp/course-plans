# Lab Notes 10 — Grounded STRIPS and Partial-Order Planning

**Concept recap:** grounding substitutes every parameter combination into a schema's templates to
produce concrete actions; a partial-order plan adds steps only for open preconditions, recording
causal links, and repairs threats by adding an ordering constraint rather than a full linear order.

**Common pitfalls:**
- Using `itertools.product` instead of `itertools.permutations` (or vice versa, depending on
  whether a schema's parameters may repeat the same object) when grounding — a `Move(x, y, z)`
  schema should usually disallow `x == y` or `y == z`; decide and filter deliberately rather than
  grounding nonsensical actions like "move a block from itself to itself."
- Forgetting that a causal link, once added, must be checked against *every other* step in the
  plan (including ones added later) for new threats — a common bug only checks threats at the
  moment a causal link is created and never re-checks after subsequent steps are added.
- Resolving a threat by reordering steps that are *already* ordered the other way by an existing
  causal link — always check `plan.is_ordered` before adding a new ordering, to avoid creating a
  contradictory (cyclic) set of ordering constraints.
- In the planning-graph exercise, forgetting "persistence" (no-op) actions that carry an
  unchanged proposition forward to the next level — omitting these makes facts disappear between
  levels even when nothing deleted them, which silently breaks later mutex reasoning.

**Debugging tip:** print the full set of ordering constraints and causal links after each
`pop_resolve_*` call; a plan with a cycle in its orderings (step A before B, B before A) signals
a threat-resolution bug, not a valid partial-order plan.

**Instructor tip:** require Task C's threat to be a genuine one found through the actual planning
process (not hand-asserted) — students who construct the threat scenario carefully tend to
understand causal links far better than those who are simply given one pre-built.

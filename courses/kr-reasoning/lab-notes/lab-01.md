# Lab Notes 1 — A Mini-World Triple Store

**Concept recap:** a triple store represents knowledge as a flat set of `(subject, predicate,
object)` facts; `query()` supports pattern matching with `None` as a wildcard over any of the
three positions.

**Common pitfalls:**
- Using a `dict` keyed by subject instead of a flat list, which quietly breaks once a subject has
  more than one fact under the same predicate (e.g., a student completing several courses) —
  prefer a flat list of tuples, or a list-valued dict, from the start.
- Forgetting that `query()` must treat `None` as "match anything," not as a literal value to
  compare against — a common bug is `t[0] == subject` even when `subject is None`, which then only
  matches facts whose subject is literally `None`.
- Conflating "the triple store has no fact that X" with "X is false" — at this stage the store is
  silent on anything not explicitly listed; this distinction becomes important again in Week 9
  (closed-world assumption) and should not be assumed either way yet.

**Debugging tip:** print `len(kb.triples)` after each batch of `add()` calls while building the
mini-world — a silently-dropped triple (e.g., from a copy-paste subject typo) is far easier to
catch by count than by scanning printed output.

**Instructor tip:** have students build their mini-world domain *before* writing queries, and
require at least one predicate relating two non-trivial entity types (not just "X has-property
Y") — a store with only unary-style facts does not yet expose why relational knowledge (Week 3's
motivation for first-order logic) will eventually be needed.

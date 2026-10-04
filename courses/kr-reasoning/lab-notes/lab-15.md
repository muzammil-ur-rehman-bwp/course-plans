# Lab Notes 15 — A Knowledge-Graph Triple Store with Pattern Queries

**Concept recap:** `path_query` follows a sequence of predicates hop by hop, starting from one
node and accumulating the set of nodes reachable after each hop — a direct multi-hop
generalization of the single-predicate `query` method.

**Common pitfalls:**
- Implementing `path_query` so that it accumulates *all* nodes visited across every hop, rather
  than only the current hop's frontier — the function must track `current` as exactly the set of
  nodes reached by the *previous* hop, replacing it at each step, not growing it indefinitely.
- Forgetting that a node with no outgoing edge for the next predicate in the pattern correctly
  contributes nothing (an empty set) to the next frontier — this is expected behavior, not a bug,
  and should not raise an error.
- Building Task B's graph without a genuine 2+-hop chain available (e.g., every predicate only
  ever appears once per subject with no onward edge) — this makes Task C's queries trivially
  single-hop in practice; deliberately include at least one real chain of 2+ predicates.
- In Task D, under-specifying the semantic-network rebuild so the comparison is unfair (e.g.,
  comparing a 15-triple knowledge graph against a 2-node semantic network) — rebuild a
  comparably-sized fragment so the comparison reflects the representations, not just scale.

**Debugging tip:** print the frontier set after each hop inside `path_query` on a small test
graph; an unexpectedly large or empty frontier at some hop immediately localizes whether the bug
is in `query`'s wildcard matching or in how `path_query` carries the frontier forward.

**Instructor tip:** ask students, before grading Task D, to state out loud which single feature
(multi-hop traversal, default-overriding inheritance, or something else) each representation
supports more directly — a comparison that only discusses syntax ("triples vs. objects") without
naming a concrete capability difference has missed the point of the exercise.

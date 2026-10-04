# Lab Notes 14 — An Integrated Knowledge-Based Agent

**Concept recap:** `_expand_taxonomic_facts` turns frame slots/parent links into plain string
facts the rule engine can match as premises; `ask_forward`/`ask_backward` are the Week 5 engine
functions, now methods on a class that also owns frames and a trace dict.

**Common pitfalls:**
- Calling `_expand_taxonomic_facts` only once, before any frames are added, instead of at the
  start of every `ask_forward`/`ask_backward` call — if frames are added after agent
  construction (a common pattern in Task B), stale or missing taxonomic facts silently cause
  rules that should fire not to fire.
- Forgetting to record a trace entry for facts expanded from frames (treating them as plain
  "given" facts is acceptable, but they must appear in `trace` at all) — otherwise `explain`
  cannot walk back past a frame-derived premise.
- Re-using the exact same `visited` set across multiple separate `ask_backward` calls (e.g., by
  giving it a mutable default argument) — Python's default-argument-is-shared-across-calls
  pitfall here causes a goal proved unreachable in one query to be incorrectly treated as still
  "in progress" in a later, unrelated query; always default to `None` and create a fresh set
  inside the function body, exactly as shown in the lecture content.
- Querying with `strategy="forward"` and `strategy="backward"` against two different copies of
  the fact set (e.g., one stale from before new facts were added) — both must read from the
  *same* `self.facts`/`self.frames` state for the Task C agreement check to be meaningful.

**Debugging tip:** before trusting a backward-chaining trace, independently confirm via
`ask_forward` that the full derived fact set contains the same goal — persistent disagreement
between the two strategies almost always traces back to `_expand_taxonomic_facts` being called
inconsistently between the two code paths.

**Instructor tip:** have students build Task B's toy domain so that at least one rule's premise
can *only* be satisfied via a frame-derived fact (not a plain `add_fact` call) — this forces a
genuine integration test rather than a rule base that happens to work without ever touching the
frame-expansion code path.

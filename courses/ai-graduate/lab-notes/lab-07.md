# Lab Notes 7 — Relaxed Planning Graphs & HTN Decomposition

**Concept recap:** the relaxed planning graph ignores delete lists, growing monotonically until
the goal literals appear or the graph levels off; h_level is the layer index at which all goal
literals first co-occur. HTN planning decomposes abstract tasks into subtasks via
domain-authored methods.

**Common pitfalls:**
- Forgetting to ignore delete lists in the relaxed graph — accidentally removing literals when
  "applying" an action turns Task A into the (much more expensive) full planning-graph
  construction with mutexes, not the cheap relaxed version this lab asks for.
- Returning `None` from `build_relaxed_planning_graph` when the graph levels off, but then
  treating that `None` as `h_level = 0` in Task B's ordering check — a goal that is unreachable
  even in the relaxed problem must be handled as a distinct case, not silently coerced to zero.
- In Task C/D, a method whose subtask list calls itself (directly or indirectly) without a base
  case reaching a primitive task causes unbounded recursion — always include a primitive-task
  base case in every method chain.
- Assuming the *first* method that type-checks is always the "best" decomposition — `htn_decompose`
  as written just takes the first method whose subtasks all succeed; Task D's two-method setup
  is designed to show this is a depth-first choice, not a search for an optimal decomposition.

**Debugging tip:** print the planning-graph layers one at a time for a small instance and
manually verify, by hand, that each new layer is exactly the union of the previous layer and
every enabled action's add-effects — this catches "forgot to ignore delete lists" bugs
immediately.

**Instructor tip:** ask students to explain, in one sentence, why HTN's `Deliver` task with two
methods is not itself doing search over "reachable states" the way flat STRIPS planning does —
it is choosing among a small, domain-authored set of decompositions instead.

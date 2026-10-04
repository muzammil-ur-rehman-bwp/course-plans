# Lab Notes 9 — Classical Planning: A Toy STRIPS Planner

**Concept recap:** a STRIPS action is applicable when its preconditions are a subset of the
current state; applying it removes the delete-list and adds the add-list; a plan is found by
searching over states reachable via applicable actions, exactly like Weeks 3–4's search but with
successors generated from action schemas instead of a hand-written `result()`.

**Common pitfalls:**
- Forgetting that `state - delete_list | add_list` must happen in that order (delete first,
  then add) — reversing the order can incorrectly keep or drop facts when add/delete lists
  overlap.
- Writing an action schema whose preconditions don't actually guard against an invalid
  application (e.g., allowing `Stack(x, y)` when `y` is not clear) — always double check
  preconditions against the domain's physical constraints.
- Using a mutable `set` as a dict/frozenset key for the `visited` set in the forward search —
  always wrap states in `frozenset(...)` before using them as keys, as shown in lecture.

**Debugging tip:** after the planner returns a plan, re-apply every action in sequence from the
initial state by hand (or with a small verification loop) and confirm the final state actually
contains the goal facts — a planner can have a subtle bug that still returns *a* plan, just not
a correct one.

**Instructor tip:** have students manually trace just the first 2 states expanded by the
forward-search planner on the multi-step blocks-world goal before trusting the full run — this
catches most action-schema bugs (wrong preconditions/effects) before they're buried in a larger
search trace.

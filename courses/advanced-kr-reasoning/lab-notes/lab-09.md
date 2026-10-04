# Lab Notes 9 — Explicit-State LTL Model Checker

**Concept recap:** `M ⊨ φ` holds iff no reachable accepting cycle exists in the product with
a Büchi automaton for ¬φ; this lab's checker is an explicit-state, teaching-scale stand-in using
direct cycle search over a small state graph, not the general automata-theoretic construction.

**Common pitfalls:**
- Searching for a cycle over the **entire** state graph instead of restricting to states
  consistent with the property under test (e.g., `bad_cycle_states` in the liveness checker) —
  this can report a spurious violation from a cycle that has nothing to do with the property, or
  miss a real one buried among irrelevant states.
- Off-by-one errors in `find_cycle_through`'s stack-slicing (`stack_path[stack_path.index(nxt):]`)
  — printing the returned cycle and manually checking it is actually a cycle (first and last
  states connected by a transition) is the fastest sanity check.
- In Task C, building a system where the flaw is *not actually reachable* from any initial state
  — a classic example-construction bug; always confirm the flawed cycle's states appear in
  `reachable(kripke, kripke.initial)` before trusting a reported violation (or its absence).

**Debugging tip:** before testing the full liveness checker, manually trace by hand which states
belong to `bad_cycle_states` for your Task C system, and compare against what the code computes —
a wrong `bad_cycle_states` set makes every downstream result meaningless regardless of how correct
the cycle-search code itself is.

**Instructor tip:** explicitly ask students to state, in one sentence, what this lab's checker
does differently from the full automata-theoretic approach (no explicit Büchi automaton or
product construction) — this guards against students walking away believing they have
implemented "the" LTL model-checking algorithm rather than a scoped, correct-for-this-fragment
stand-in.

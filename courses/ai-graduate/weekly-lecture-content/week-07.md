# Week 7 — Lecture Content: Rigorous Classical Planning

## 1. The Computational Complexity of Classical Planning
Classical (STRIPS-style) planning problems ask: given an initial state, a goal condition, and a
set of actions each with preconditions and effects (add/delete lists), does there exist a
sequence of actions transforming the initial state into a state satisfying the goal?

**Result (stated, with intuition — not proved in full).** Plan existence for general STRIPS
planning is **PSPACE-complete**. Intuition for membership in PSPACE: a nondeterministic algorithm
can guess a plan one action at a time and check it incrementally using only polynomial space
(the current state plus a counter), since it never needs to remember the whole plan at once —
and PSPACE = NPSPACE (Savitch's theorem), so this nondeterministic polynomial-space procedure
implies a deterministic polynomial-space one exists too. Intuition for PSPACE-hardness: STRIPS
planning can simulate the computation of a polynomial-space-bounded Turing machine, encoding each
tape configuration as a world state and each transition as an action — since verifying a
plan can require reasoning about exponentially many reachable states in the worst case (unlike
SAT, where a candidate solution can simply be checked in polynomial time), planning is believed
strictly harder than NP-complete problems. Formally, NP ⊆ PSPACE, and this containment is
believed (though, like P vs. NP, not proven) to be strict.

**Why this matters practically.** Because plan existence is PSPACE-complete rather than merely
NP-complete, there is comparatively less hope that clever encoding tricks (e.g., encoding
planning as SAT, which works well for *bounded-length* plan existence — itself NP-complete for a
fixed horizon) will give efficient general solvers; efficient planning in practice relies heavily
on domain structure, heuristics, and problem-specific restrictions, which is exactly what the
rest of this week's material (planning graphs, HTN decomposition) provides.

## 2. The Planning Graph
A **planning graph** is a layered graph alternating **literal layers** (propositions that might
be true) and **action layers** (actions whose preconditions are satisfied by the previous literal
layer), built forward from the initial state. Layer 0 is the set of literals true in the initial
state. Action layer i contains every action whose preconditions are all present in literal layer
i; literal layer i+1 contains every literal that is the initial-state literals plus every effect
of an action in action layer i. The graph is built until no new literals appear (it "levels off")
or the goal literals all appear together in some layer.

## 3. The Relaxed-Planning-Graph Heuristic
Building the full planning graph with mutex (mutual exclusion) tracking is itself expensive; a
cheaper and still very informative heuristic is the **relaxed-plan heuristic**: build the
planning graph while **ignoring delete lists** (the "relaxed" problem, where actions only ever
add literals, never remove them). This relaxation makes the problem easier because once a
literal becomes true, it stays true, which also makes the planning graph monotonically growing
and easy to build. The heuristic value h(s) for a state s is the number of actions in an
extracted relaxed plan (a greedy backward extraction from the goal layer through the levelled-off
relaxed planning graph) — or, more simply, the index of the first layer in which all goal
literals appear. This heuristic is not admissible in general for non-admissible variants but the
standard levels-based version (h_level) is admissible, and is widely used (e.g., in the
FF/HSP-style family of planners) to guide heuristic-search planning efficiently in practice,
despite the problem's PSPACE-complete worst case.

```python
def build_relaxed_planning_graph(initial_literals, actions, goal_literals, max_layers=50):
    """actions: list of (name, preconditions: set, add_effects: set).
    Returns the layer index at which all goal_literals first appear, or None if it levels off
    without reaching the goal (goal unreachable even in the relaxed, delete-free problem)."""
    layer = set(initial_literals)
    seen_layers = [frozenset(layer)]
    for i in range(max_layers):
        if goal_literals <= layer:
            return i
        applicable = [a for a in actions if a[1] <= layer]
        if not applicable:
            return None  # no progress possible
        new_layer = set(layer)
        for _, _, add_effects in applicable:
            new_layer |= add_effects
        if new_layer == layer:
            return None  # levelled off without reaching the goal
        layer = new_layer
        seen_layers.append(frozenset(layer))
    return None

# Example usage: h(s) = build_relaxed_planning_graph({...}, actions, {"goal_literal"})
```

## 4. Hierarchical Task Network (HTN) Planning
HTN planning represents a plan not as a flat sequence of primitive actions found by blind search,
but as a hierarchy: **abstract (compound) tasks** are decomposed by **methods** into subtasks
(which may themselves be abstract or primitive), recursively, until only primitive actions
remain. For example, an abstract task `Travel(A, B)` might have a method decomposing it into
`[Drive(A, Airport), Fly(Airport, DestAirport), Drive(DestAirport, B)]`. HTN planning exploits
domain structure that flat STRIPS search ignores: the domain author encodes known-good task
decompositions directly, which prunes the search space enormously compared to blind forward
search, at the cost of requiring a human-authored task hierarchy and methods library.

```python
def htn_decompose(task, methods, primitives):
    """task: a task name (string). methods: dict task -> list of possible subtask lists
    (each a list of task names). primitives: set of primitive task names.
    Returns one valid primitive-action sequence via depth-first method expansion."""
    if task in primitives:
        return [task]
    for subtasks in methods.get(task, []):
        plan = []
        ok = True
        for sub in subtasks:
            sub_plan = htn_decompose(sub, methods, primitives)
            if sub_plan is None:
                ok = False
                break
            plan.extend(sub_plan)
        if ok:
            return plan
    return None  # no method succeeded
```

## 5. In-Class/Lab Exercise
For a small logistics-style STRIPS domain (move a package between two locations via a vehicle),
build the relaxed planning graph by hand for two layers, compute h_level for the goal
"package at destination," and then write an HTN decomposition of the single abstract task
`Deliver(package, destination)` into the primitive `Load`/`Drive`/`Unload` actions.

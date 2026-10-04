# Week 10 — Lecture Content: Planning in Depth

## 1. STRIPS Revisited: Variables and Grounding
A brief classical-AI survey represents each STRIPS action as a single concrete object (e.g.
`Stack(A, B)`). In depth, STRIPS action **schemas** are written with **variables** and
**grounded** — instantiated with every combination of domain objects — before planning begins.

```python
class ActionSchema:
    def __init__(self, name, parameters, preconditions, add_list, delete_list):
        self.name = name
        self.parameters = parameters           # e.g. ("x", "y")
        self.preconditions = preconditions      # templates, e.g. "clear(x)"
        self.add_list = add_list
        self.delete_list = delete_list

    def ground(self, objects):
        """Yield one grounded action per assignment of domain objects to parameters."""
        import itertools
        for values in itertools.permutations(objects, len(self.parameters)):
            substitution = dict(zip(self.parameters, values))
            def subst(template):
                for var, val in substitution.items():
                    template = template.replace(var, val)
                return template
            yield GroundedAction(
                name=f"{self.name}({', '.join(values)})",
                preconditions={subst(p) for p in self.preconditions},
                add_list={subst(a) for a in self.add_list},
                delete_list={subst(d) for d in self.delete_list},
            )

class GroundedAction:
    def __init__(self, name, preconditions, add_list, delete_list):
        self.name, self.preconditions = name, preconditions
        self.add_list, self.delete_list = add_list, delete_list

    def is_applicable(self, state):
        return self.preconditions.issubset(state)

    def apply(self, state):
        return (state - self.delete_list) | self.add_list
```

Grounding `Stack(x, y)` over objects `{A, B, C}` produces every ordered pair (`Stack(A,B)`,
`Stack(B,A)`, `Stack(A,C)`, ...) as a separate `GroundedAction` — exactly the action set a forward
planner searches over, but now generated automatically from one schema, reusable across domains
of any size, rather than hand-written per problem instance.

## 2. Partial-Order Planning (POP)
Forward state-space search (as covered briefly elsewhere) commits to a **total order** of actions
as it searches. **Partial-order planning** instead builds a plan as a partially ordered set of
steps, adding steps only to satisfy specific open preconditions, and ordering two steps relative
to each other only when forced to. Key concepts:
- An **open precondition** is a precondition of some step not yet guaranteed by an earlier step.
- A **causal link** `Si --p--> Sj` records that step `Si` achieves precondition `p` needed by
  step `Sj`.
- A **threat** is a third step `Sk` that could undo `p` (deletes `p`) and is not yet ordered
  safely relative to `Si` and `Sj`; it must be **resolved** by ordering `Sk` before `Si` or after
  `Sj` (demotion/promotion).

```python
def pop_resolve_open_precondition(plan, open_precondition, step, available_actions):
    """Find or add a step that achieves open_precondition, needed by step, add a causal link."""
    for existing_step in plan.steps:
        if open_precondition in existing_step.add_list:
            plan.add_causal_link(existing_step, step, open_precondition)
            plan.add_ordering(existing_step, step)
            return
    for action in available_actions:
        if open_precondition in action.add_list:
            plan.add_step(action)
            plan.add_causal_link(action, step, open_precondition)
            plan.add_ordering(action, step)
            return
    raise ValueError(f"no action can achieve {open_precondition}")

def pop_resolve_threats(plan):
    """For every causal link Si--p-->Sj and every other step Sk that deletes p and is not
    already ordered before Si or after Sj, add an ordering constraint to remove the threat."""
    for link in plan.causal_links:
        for step in plan.steps:
            if step in (link.si, link.sj):
                continue
            if link.p in step.delete_list and not plan.is_ordered(step, link.si) and not plan.is_ordered(link.sj, step):
                plan.add_ordering(step, link.si)  # demote: force step before si (one valid repair)
```

POP's benefit over committing to a total order early: two steps with no causal dependency never
need to be ordered relative to each other at all, which avoids ruling out valid interleavings and
can expose more opportunities for the search to recognize reusable partial plans across subgoals.

## 3. Planning Graphs (Conceptual Introduction to GraphPlan)
A **planning graph** alternates **proposition levels** (`P0, P1, P2, ...`, the facts possibly
true after 0, 1, 2, ... plan steps) and **action levels** (`A0, A1, ...`, the actions whose
preconditions are satisfied at the preceding proposition level). It is built forward,
level-by-level, and tracks **mutex** (mutual exclusion) relations: two actions at the same level
are mutex if they interfere (one deletes the other's precondition or effect, or they have
inconsistent preconditions); two propositions at the same level are mutex if every way of
achieving one is mutex with every way of achieving the other.

```python
def next_proposition_level(prop_level, actions):
    """One level of planning-graph expansion: every action applicable given prop_level
    contributes its add-list to the next proposition level (plus 'persistence' no-op actions
    carrying every unchanged proposition forward, omitted here for brevity)."""
    applicable = [a for a in actions if a.preconditions.issubset(prop_level)]
    next_props = set(prop_level)
    for action in applicable:
        next_props |= action.add_list
    return next_props, applicable
```

**GraphPlan** expands the graph level by level until all goal propositions appear in a level
*without* being pairwise mutex, then searches **backward** from that level, at each step picking
a set of non-mutex actions whose combined add-lists cover the current goals, recursing on their
combined preconditions as the new goals one level earlier. This course introduces the idea and
the graph-construction step above; a full backward-search extraction implementation is beyond
this week's lab, left as a pointer to further study.

## 4. In-Class Exercise
Ground the schema `Move(x, y, z)` (move block `x` from `y` to `z`) over objects `{A, B, C, Table}`
by hand (list the first 5 grounded actions produced); then, for a 2-step plan with one causal
link, identify a hypothetical third action that would threaten it and state which repair
(promotion or demotion) resolves the threat.

## 5. Capstone Kickoff
The capstone project (introduced this week, proposal due Week 11) asks you to integrate **at
least two formalisms** from this course into one system. A STRIPS planner whose initial state is
first derived by a Week 9 default-reasoning step is one suggested direction — see
`assignments/capstone-proposal-guidelines.md`.

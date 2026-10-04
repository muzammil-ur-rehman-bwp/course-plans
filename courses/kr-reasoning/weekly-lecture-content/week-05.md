# Week 5 — Lecture Content: Rule-Based Systems

## 1. Production Systems
A **production system** has three parts: a **working memory** of current facts, a **rule base**
of condition-action ("production") rules of the form "if premises hold, then conclusion holds,"
and an inference engine that runs a **recognize-act cycle**: find rules whose premises are
satisfied by working memory (recognize), pick one if several apply (**conflict resolution** —
e.g., rule priority, specificity, or recency; treated briefly here), and fire it, updating working
memory (act). This is the architecture behind classical expert systems (e.g., MYCIN).

## 2. A Domain-Independent Rule Representation
Where Week 2's Horn-clause example hard-coded one rule base into the chaining functions, this
week builds a **reusable** representation and engine that any rule base can plug into.

```python
from dataclasses import dataclass
from typing import FrozenSet

@dataclass(frozen=True)
class Rule:
    premises: FrozenSet[str]
    conclusion: str

    def is_applicable(self, facts: set) -> bool:
        return self.premises.issubset(facts)
```

## 3. Forward Chaining (Data-Driven)
Repeatedly fire any rule whose premises are already known, adding its conclusion, until a fixed
point is reached (nothing new fires) or the query is derived.

```python
def forward_chain(facts, rules, query=None):
    facts = set(facts)
    derived_by = {}  # fact -> rule that derived it, for an explanation trace
    changed = True
    while changed:
        changed = False
        for rule in rules:
            if rule.conclusion not in facts and rule.is_applicable(facts):
                facts.add(rule.conclusion)
                derived_by[rule.conclusion] = rule
                changed = True
    if query is not None:
        return query in facts, facts, derived_by
    return facts, derived_by
```

## 4. Backward Chaining (Goal-Directed)
Start from the query and recursively ask whether its premises can be established — either because
they are already known facts, or because some rule concludes them (whose premises must then
themselves be established).

```python
def backward_chain(facts, rules, goal, visited=None):
    if visited is None:
        visited = set()
    if goal in facts:
        return True
    if goal in visited:
        return False  # avoid infinite recursion on a rule cycle
    visited.add(goal)
    for rule in rules:
        if rule.conclusion == goal:
            if all(backward_chain(facts, rules, premise, visited) for premise in rule.premises):
                return True
    return False
```

## 5. Comparing the Two Strategies
Forward chaining derives *everything* reachable from the facts — efficient when many queries will
be asked against the same fact set, or when the goal is unknown in advance. Backward chaining
derives only what is needed for *one* specific query — efficient when the rule base is large but
only a narrow query is of interest. Both are sound and complete over the same rule-base
representation; which to use is an engineering choice, not a correctness one.

## 6. A Reusable Engine Over Two Different Domains
The point of decoupling `Rule`/`forward_chain`/`backward_chain` from any specific domain is that
the *same* engine code now runs over unrelated rule bases without modification:

```python
animal_rules = [
    Rule(frozenset({"has_feathers"}), "is_bird"),
    Rule(frozenset({"is_bird", "cannot_fly", "swims"}), "is_penguin"),
]
fault_rules = [
    Rule(frozenset({"engine_wont_start", "no_clicking_sound"}), "dead_battery"),
    Rule(frozenset({"engine_wont_start", "clicking_sound"}), "starter_motor_fault"),
]

print(forward_chain({"has_feathers", "cannot_fly", "swims"}, animal_rules, "is_penguin")[0])
print(backward_chain({"engine_wont_start", "clicking_sound"}, fault_rules, "starter_motor_fault"))
```

Both calls run through the exact same `forward_chain`/`backward_chain` functions — only the facts
and rules passed in differ, which is the hallmark of a genuinely reusable KR engine rather than a
one-off script.

## 7. In-Class Exercise
Given a 6-rule knowledge base (provided on the board) and an initial fact set, trace forward
chaining by hand to find every derivable fact, then trace backward chaining for one specific goal
fact and confirm it succeeds exactly when forward chaining also derives it.

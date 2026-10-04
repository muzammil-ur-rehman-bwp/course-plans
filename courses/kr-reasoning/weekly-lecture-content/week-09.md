# Week 9 — Lecture Content: Non-Monotonic Reasoning

## 1. Midterm Exam
Covers Weeks 1–8: KR desiderata; propositional logic (normal forms, resolution, SAT); first-order
logic (syntax, semantics, translation); FOL inference (unification, resolution, Skolemization);
rule-based systems; semantic networks and frames; description logics and ontologies; constraint
satisfaction (AC-3, backtracking heuristics). See the Week 8 review roadmap for the full topic
list and practice problems.

## 2. Monotonicity, and Why It Is a Problem
Classical logic is **monotonic**: if `KB ⊨ α`, then for *any* additional sentence `β`,
`KB ∪ {β} ⊨ α` still holds — nothing already proved can ever be taken back by learning something
new. This is mathematically clean but wrong as a model of everyday reasoning: told "Tweety is a
bird," we conclude "Tweety can fly"; told moments later "Tweety is a penguin," we *retract* that
conclusion — something monotonic entailment structurally cannot do. Week 6 patched this at the
representation level (frame defaults); this week formalizes the underlying reasoning pattern.

## 3. The Closed-World Assumption (CWA)
Under the **open-world assumption** (the default for classical FOL), a fact not stated in the KB
is simply *unknown* — neither asserted true nor false. Under the **closed-world assumption**,
any atomic fact not derivable from the KB is treated as **false**. This is how most databases and
rule engines (including Weeks 5's forward/backward chaining) implicitly behave: if `forward_chain`
does not derive `Eligible(bob)`, a CWA-based system concludes `¬Eligible(bob)`, not merely "it is
unknown whether Bob is eligible."

```python
def cwa_query(facts, rules, atom):
    """Closed-world query: True if derivable, False otherwise (never 'unknown')."""
    derived = set(facts)
    changed = True
    while changed:
        changed = False
        for premises, conclusion in rules:
            if conclusion not in derived and all(p in derived for p in premises):
                derived.add(conclusion)
                changed = True
    return atom in derived  # anything not derived is simply treated as false
```

## 4. Default Logic
**Default logic** (Reiter) formalizes defeasible rules explicitly. A **default** has the form
`prerequisite : justification / consequent` — read "if the prerequisite holds, and the
justification is consistent with what we currently believe, conclude the consequent." Unlike a
strict rule, a default's conclusion can later be blocked if the justification stops being
consistent (e.g., a more specific fact contradicts it).

```python
class Default:
    def __init__(self, prerequisite, justification, consequent):
        self.prerequisite = prerequisite    # a fact that must be known
        self.justification = justification  # a fact that must NOT be known to be false
        self.consequent = consequent        # what gets concluded if both hold

def apply_defaults(facts, defaults, strict_rules):
    """Build a default-logic extension: apply strict rules to a fixed point first, then apply
    any default whose prerequisite is derived and whose justification is not contradicted,
    repeating until no more defaults or rules apply."""
    derived = set(facts)
    changed = True
    while changed:
        changed = False
        for premises, conclusion in strict_rules:
            if conclusion not in derived and all(p in derived for p in premises):
                derived.add(conclusion)
                changed = True
        for default in defaults:
            blocked = ("not_" + default.justification) in derived  # simple negation-as-a-fact convention
            if (default.prerequisite in derived and default.consequent not in derived
                    and not blocked):
                derived.add(default.consequent)
                changed = True
    return derived
```

### Worked Example: Retracting a Conclusion
```python
bird_flies = Default(prerequisite="bird", justification="can_fly", consequent="can_fly")

facts = {"bird"}
result1 = apply_defaults(facts, [bird_flies], strict_rules=[])
print("can_fly" in result1)  # True -- the default applies; nothing blocks it yet

facts2 = {"bird", "penguin", "not_can_fly"}  # new information blocks the justification
result2 = apply_defaults(facts2, [bird_flies], strict_rules=[])
print("can_fly" in result2)  # False -- the SAME default rule no longer fires once blocked
```
This is the core non-monotonic behavior: the exact same rule set, given additional information
(`"not_can_fly"`), produces a *smaller* set of conclusions than before — something no strict rule
or resolution-based KB (Weeks 2, 4, 5) can ever do, since adding facts there can only grow, never
shrink, the derivable set.

## 5. Circumscription (Conceptual)
**Circumscription** (McCarthy) is a second non-monotonic formalism, treated here conceptually
rather than with a full implementation. Its idea: minimize the extension of certain predicates
(typically an "abnormality" predicate) consistent with what is known — formally, prefer models
where as few things as possible are abnormal. "Tweety can fly" follows because circumscription
prefers the model where Tweety is *not* abnormal-as-a-flyer; learning "Tweety is a penguin" rules
out that preferred model (penguins are stipulated abnormal-as-flyers), so the preferred remaining
models are exactly those where Tweety does not fly — the same retraction behavior as default
logic, reached through minimizing abnormality instead of blocking a justification.

## 6. In-Class Exercise
Given the default "a car with no reported fault runs : consistent with running / runs" and facts
`{car, no_reported_fault}`, apply the default to conclude `runs`; then add the fact
`not_runs` (representing a newly reported fault) and show the default no longer fires, tracing
exactly which check in `apply_defaults` blocks it.

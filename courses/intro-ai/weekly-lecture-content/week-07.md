# Week 7 — Lecture Content: Propositional Logic Inference

## 1. Entailment and Model Checking
A knowledge base `KB` **entails** a sentence `α` (written `KB ⊨ α`) if `α` is true in every
model in which `KB` is true. The simplest inference procedure, **model checking**, enumerates
all models and checks this directly — correct, but exponential in the number of symbols.

```python
import itertools

def entails(kb_sentences, alpha, symbols):
    for values in itertools.product([False, True], repeat=len(symbols)):
        model = dict(zip(symbols, values))
        if all(evaluate(s, model) for s in kb_sentences):  # evaluate() from Week 6
            if not evaluate(alpha, model):
                return False
    return True
```

## 2. Conjunctive Normal Form (CNF)
Resolution requires sentences in CNF: a conjunction of clauses, each clause a disjunction of
literals. Conversion steps: eliminate `↔` and `→`, push negations inward (De Morgan's), then
distribute `∨` over `∧`. We will represent a CNF sentence directly as a list of clauses, each
clause a set of literals (a string, or a string prefixed with `~` for negation), to keep the
resolution code below simple.

```python
# Example: (P -> Q) in CNF is just the single clause {~P, Q}
clause1 = {"~P", "Q"}
```

## 3. The Resolution Rule
From two clauses containing complementary literals, resolution derives a new clause with those
literals removed:
`{A, B}` and `{~B, C}` resolve to `{A, C}`.

**Resolution refutation**: to prove `KB ⊨ α`, add `¬α` to the KB (as clauses) and show the
resulting clause set is unsatisfiable (resolution derives the empty clause).

```python
def resolve(clause1, clause2):
    resolvents = []
    for literal in clause1:
        complement = literal[1:] if literal.startswith("~") else "~" + literal
        if complement in clause2:
            new_clause = (clause1 - {literal}) | (clause2 - {complement})
            resolvents.append(new_clause)
    return resolvents

def resolution_refutation(clauses, query_literal):
    clauses = list(clauses) + [{"~" + query_literal}]  # negate the query
    new = set()
    while True:
        pairs = [(clauses[i], clauses[j])
                 for i in range(len(clauses)) for j in range(i + 1, len(clauses))]
        for ci, cj in pairs:
            for resolvent in resolve(ci, cj):
                if not resolvent:
                    return True  # derived the empty clause: KB |= query
                new.add(frozenset(resolvent))
        if new.issubset(frozenset(c) for c in clauses):
            return False  # no new clauses: cannot prove it
        clauses.extend(set(c) for c in new)
```

## 4. Horn Clauses, Forward Chaining, Backward Chaining
A **definite (Horn) clause** has exactly one positive literal, e.g. `P ∧ Q → R`. Horn-clause
knowledge bases support two efficient, complete inference strategies:

- **Forward chaining** (data-driven): repeatedly fire any rule whose premises are all already
  known, adding its conclusion, until the query is derived or nothing new follows.
- **Backward chaining** (goal-driven): start from the query and recursively ask whether its
  premises can be established, either because they are known facts or because some rule
  concludes them.

```python
def forward_chain(facts, rules, query):
    facts = set(facts)
    changed = True
    while changed:
        changed = False
        for premises, conclusion in rules:
            if conclusion not in facts and all(p in facts for p in premises):
                facts.add(conclusion)
                changed = True
    return query in facts
```

## 5. Worked Example
KB: `Rain -> WetGrass`, `Sprinkler -> WetGrass`, facts `{Rain}`. Query: `WetGrass`.
- Forward chaining: `Rain` is known, the rule `Rain -> WetGrass` fires, `WetGrass` is added.
  Query succeeds.
- Backward chaining: to prove `WetGrass`, try each rule concluding it; `Rain -> WetGrass`
  succeeds because `Rain` is a known fact.

## 6. In-Class Exercise
Given the KB above plus `Sprinkler` as an unknown fact, trace forward chaining and backward
chaining by hand for the query `WetGrass`, and confirm both reach the same (correct) answer
using only the `Rain` fact.

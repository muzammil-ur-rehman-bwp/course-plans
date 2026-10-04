# Week 6 — Lecture Content: Knowledge Representation & Propositional Logic

## 1. Why Knowledge Representation?
Search (Weeks 3–5) requires a state space we can enumerate or generate. Many problems instead
require representing general facts and rules ("all birds fly, except penguins") and deriving new
facts from them. Logic gives us a precise syntax (what sentences look like) and semantics
(what they mean, and when they are true).

## 2. Propositional Logic Syntax
Built from propositional symbols (`P`, `Q`, ...) and connectives:
- `¬P` (negation), `P ∧ Q` (conjunction), `P ∨ Q` (disjunction),
  `P → Q` (implication), `P ↔ Q` (biconditional).

A **sentence** is either an atomic symbol or a connective applied to smaller sentences.

## 3. Semantics & Truth Tables
A **model** assigns `True`/`False` to every propositional symbol. The truth value of a complex
sentence under a model is computed from its connectives:

| P | Q | ¬P | P∧Q | P∨Q | P→Q | P↔Q |
|---|---|---|---|---|---|---|
| T | T | F | T | T | T | T |
| T | F | F | F | T | F | F |
| F | T | T | F | T | T | F |
| F | F | T | F | F | T | T |

```python
import itertools

def evaluate(expr, model):
    """expr is a nested tuple: ('not', a) | ('and', a, b) | ('or', a, b) |
       ('implies', a, b) | ('iff', a, b) | a bare symbol string."""
    if isinstance(expr, str):
        return model[expr]
    op, *args = expr
    if op == "not":
        return not evaluate(args[0], model)
    a = evaluate(args[0], model)
    b = evaluate(args[1], model)
    if op == "and":
        return a and b
    if op == "or":
        return a or b
    if op == "implies":
        return (not a) or b
    if op == "iff":
        return a == b
    raise ValueError(f"unknown operator {op}")

def truth_table(expr, symbols):
    rows = []
    for values in itertools.product([False, True], repeat=len(symbols)):
        model = dict(zip(symbols, values))
        rows.append((model, evaluate(expr, model)))
    return rows

for model, result in truth_table(("implies", "P", "Q"), ["P", "Q"]):
    print(model, "->", result)
```

## 4. Satisfiability & Validity
- A sentence is **satisfiable** if it is true in at least one model.
- A sentence is **valid** (a tautology) if it is true in every model — e.g. `P ∨ ¬P`.
- A sentence is **unsatisfiable** (a contradiction) if it is false in every model.

```python
def is_valid(expr, symbols):
    return all(result for _, result in truth_table(expr, symbols))

def is_satisfiable(expr, symbols):
    return any(result for _, result in truth_table(expr, symbols))

print(is_valid(("or", "P", ("not", "P")), ["P"]))        # True  (law of excluded middle)
print(is_satisfiable(("and", "P", ("not", "P")), ["P"]))  # False (contradiction)
```

## 5. Logical Equivalence
Two sentences are **logically equivalent** if they have the same truth value in every model.
Useful equivalences:
- Implication elimination: `P → Q ≡ ¬P ∨ Q`
- De Morgan's: `¬(P ∧ Q) ≡ ¬P ∨ ¬Q`, and `¬(P ∨ Q) ≡ ¬P ∧ ¬Q`
- Double negation: `¬¬P ≡ P`

```python
def are_equivalent(expr1, expr2, symbols):
    t1 = [r for _, r in truth_table(expr1, symbols)]
    t2 = [r for _, r in truth_table(expr2, symbols)]
    return t1 == t2

print(are_equivalent(("implies", "P", "Q"), ("or", ("not", "P"), "Q"), ["P", "Q"]))  # True
```

## 6. Worked Example
Prove `(P → Q) ↔ (¬P ∨ Q)` is a tautology by building its full truth table and checking every
row evaluates to `True` — the code above confirms this mechanically; the point of doing it by
hand first is to see *why* implication elimination is a safe rewriting rule, not just a
memorized formula.

## 7. In-Class Exercise
Build the truth table for `¬(P ∧ Q) ↔ (¬P ∨ ¬Q)` by hand and confirm it is a tautology (De
Morgan's law), then verify with `truth_table`.

# Week 2 — Lecture Content: Propositional Logic in Depth

## 1. Recap: Syntax and Semantics
A propositional sentence is built from atomic symbols (`P`, `Q`, ...) and connectives (`¬`, `∧`,
`∨`, `→`, `↔`). A **model** is an assignment of `True`/`False` to every symbol; a sentence is
**satisfiable** if some model makes it true, **valid** (a tautology) if every model makes it true,
and **unsatisfiable** if no model makes it true.

## 2. Normal Forms
Resolution (below) requires sentences in **conjunctive normal form (CNF)**: a conjunction of
clauses, each clause a disjunction of literals. The conversion procedure:
1. **Eliminate `↔`:** `α ↔ β` becomes `(α → β) ∧ (β → α)`.
2. **Eliminate `→`:** `α → β` becomes `¬α ∨ β`.
3. **Push negation inward (De Morgan's):** `¬(α ∧ β)` becomes `¬α ∨ ¬β`; `¬(α ∨ β)` becomes
   `¬α ∧ ¬β`; double negation `¬¬α` becomes `α`.
4. **Distribute `∨` over `∧`:** `α ∨ (β ∧ γ)` becomes `(α ∨ β) ∧ (α ∨ γ)`.

**Disjunctive normal form (DNF)** is the dual — a disjunction of conjunctions of literals —
produced by the same first three steps, then distributing `∧` over `∨` instead.

```python
def to_cnf_clauses(sentence_tree):
    """Given a sentence already reduced to only not/and/or (steps 1-3 applied),
    distribute 'or' over 'and' to produce a list of clauses (each a set of literals)."""
    kind = sentence_tree[0]
    if kind == "or":
        left_clauses = to_cnf_clauses(sentence_tree[1])
        right_clauses = to_cnf_clauses(sentence_tree[2])
        # If either side is itself a conjunction, distribute; otherwise it's a single clause.
        if len(left_clauses) > 1 or len(right_clauses) > 1:
            return [l | r for l in left_clauses for r in right_clauses]
        return [left_clauses[0] | right_clauses[0]]
    if kind == "and":
        return to_cnf_clauses(sentence_tree[1]) + to_cnf_clauses(sentence_tree[2])
    if kind == "not":
        return [{"~" + sentence_tree[1]}]
    return [{sentence_tree}]  # a bare literal
```

In this course, clauses are represented directly as Python `set`s of literal strings (e.g.
`{"P", "~Q"}`), which keeps the resolution code below simple without needing a full expression
parser for every exercise.

## 3. The Resolution Rule, In Depth
From two clauses containing a pair of complementary literals (`L` in one, `~L` in the other),
resolution derives a new clause — the **resolvent** — with those two literals removed and
everything else kept:

`{A, B}` and `{~B, C}` share the complementary pair `B`/`~B`, resolving to `{A, C}`.

If a clause's *only* literal is the complement of the other clause's only literal (e.g. `{B}` and
`{~B}`), the resolvent is the **empty clause** `{}` — a contradiction, meaning the two clauses
together are unsatisfiable.

**Resolution refutation**: to prove `KB ⊨ α` (the KB entails `α`), negate `α`, add its CNF
clauses to the KB's clauses, and show the resulting clause set is unsatisfiable by repeatedly
resolving pairs of clauses until either the empty clause appears (proved) or no new clauses can
be derived (not proved — refutation-complete, so this correctly means `KB ⊭ α`).

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
    clauses = [set(c) for c in clauses] + [{"~" + query_literal}]
    new = set()
    while True:
        pairs = [(clauses[i], clauses[j])
                 for i in range(len(clauses)) for j in range(i + 1, len(clauses))]
        for ci, cj in pairs:
            for resolvent in resolve(ci, cj):
                if not resolvent:
                    return True  # empty clause derived: KB |= query
                new.add(frozenset(resolvent))
        if new.issubset(frozenset(c) for c in clauses):
            return False  # fixed point reached, no empty clause: cannot prove it
        clauses.extend(set(c) for c in new)
```

### Worked Derivation
KB clauses: `{~P, Q}` (i.e. `P → Q`), `{P}`. Query: `Q`.
1. Negate the query: add `{~Q}`.
2. Resolve `{~P, Q}` with `{~Q}` on the complementary pair `Q`/`~Q`: resolvent `{~P}`.
3. Resolve `{~P}` with `{P}` on the complementary pair `P`/`~P`: resolvent `{}` — the empty
   clause. **Proved**: `KB ⊨ Q`.

## 4. The Boolean Satisfiability Problem (SAT)
**SAT** asks: given a propositional sentence, does some model satisfy it? SAT is the canonical
**NP-complete** problem (Cook–Levin theorem) — the best known general algorithms are worst-case
exponential in the number of symbols, and no polynomial-time algorithm for SAT is known (nor is
one believed to exist, though this is unproven — the P vs. NP question). This is why a
brute-force model-checking entailment procedure (enumerate every model) and even resolution
refutation (whose worklist can grow exponentially in the worst case) do not scale to large
knowledge bases — a central, honest limitation to state plainly rather than gloss over.

```python
import itertools

def literal_true(literal, model):
    return (not model[literal[1:]]) if literal.startswith("~") else model[literal]

def is_satisfiable(clauses, symbols):
    for values in itertools.product([False, True], repeat=len(symbols)):
        model = dict(zip(symbols, values))
        if all(any(literal_true(lit, model) for lit in clause) for clause in clauses):
            return True, model
    return False, None
```

## 5. In-Class Exercise
Given the clauses `{P, Q}`, `{~P, R}`, `{~Q, R}`, `{~R}`, trace resolution refutation to determine
whether these four clauses are jointly satisfiable (hint: they are not — derive the empty clause
and identify which three resolution steps produce it).

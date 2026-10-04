# Week 4 — Lecture Content: First-Order Inference

## 1. Why Propositional Resolution Is Not Enough
Propositional resolution (Week 2) resolves two clauses when one contains literal `L` and the
other `~L` — an exact string match. FOL literals contain variables: `Likes(x, mother(x))` and
`¬Likes(john, mother(john))` are "the same, up to substitution" but are not string-identical.
**Unification** is the algorithm that finds the substitution making two expressions identical, if
one exists.

## 2. The Unification Algorithm
A **substitution** is a mapping from variables to terms, e.g. `{x: john}`. `UNIFY(e1, e2)` returns
the **most general unifier (MGU)** — the least-committal substitution making `e1` and `e2`
identical — or `None` ("failure") if no unifier exists.

```python
def unify(x, y, subst=None):
    if subst is None:
        subst = {}
    if subst is None:  # a previous failed step signalled failure
        return None
    if x == y:
        return subst
    if is_variable(x):
        return unify_var(x, y, subst)
    if is_variable(y):
        return unify_var(y, x, subst)
    if is_compound(x) and is_compound(y):
        if function_symbol(x) != function_symbol(y) or len(args(x)) != len(args(y)):
            return None
        for xi, yi in zip(args(x), args(y)):
            subst = unify(xi, yi, subst)
            if subst is None:
                return None
        return subst
    return None  # different constants, or a constant vs. a compound

def unify_var(var, x, subst):
    if var in subst:
        return unify(subst[var], x, subst)
    if is_variable(x) and x in subst:
        return unify(var, subst[x], subst)
    if occurs_check(var, x, subst):
        return None  # e.g. unifying x with f(x) would create an infinite term
    new_subst = dict(subst)
    new_subst[var] = x
    return new_subst

def occurs_check(var, x, subst):
    if var == x:
        return True
    if is_variable(x) and x in subst:
        return occurs_check(var, subst[x], subst)
    if is_compound(x):
        return any(occurs_check(var, arg, subst) for arg in args(x))
    return False
```

(`is_variable`, `is_compound`, `function_symbol`, and `args` are small helper functions over a
term representation such as nested tuples, e.g. `("mother", ("x",))` for `mother(x)`; their
implementation is part of Lab 4.)

### Worked Trace
Unify `Likes(x, mother(x))` with `Likes(john, mother(john))`:
1. Same predicate `Likes`, arity 2 — unify arguments pairwise.
2. Unify `x` with `john`: `x` is a variable → `subst = {x: john}`.
3. Unify `mother(x)` with `mother(john)`, under `subst = {x: john}`: same function symbol, unify
   arguments → unify `x` with `john` again, already consistent with `subst`.
4. Result: `{x: john}` — the MGU.

### Why the Occurs Check Matters
Unifying `x` with `f(x)` without an occurs check would produce the substitution `{x: f(x)}` — an
infinite term, since substituting it into itself never terminates (`f(x) → f(f(x)) → ...`). The
occurs check rejects this, correctly reporting failure.

## 3. Resolution for First-Order Logic
FOL resolution unifies a pair of complementary literals (one positive, one negated, same
predicate symbol) before resolving, applying the resulting substitution to the *entire* resolvent
clause — not just the resolved literals. A critical prerequisite: **variables in different
clauses must be renamed apart** before unification, since `∀x P(x)` in one clause and `∀x Q(x)` in
another use `x` to mean two unrelated universally quantified variables; failing to rename first
can cause an incorrect **variable capture**, unifying variables that were never meant to be
related.

```python
def standardize_apart(clause, suffix):
    """Rename every variable in clause by appending a unique suffix, to avoid variable capture."""
    # (a full implementation walks the clause's term tree, renaming each variable symbol)
    ...

def fol_resolve(clause1, clause2):
    resolvents = []
    for lit1 in clause1:
        for lit2 in clause2:
            if is_negation_pair(lit1, lit2):  # same predicate, opposite polarity
                subst = unify(atom_of(lit1), atom_of(lit2))
                if subst is not None:
                    new_clause = apply_subst(
                        (clause1 - {lit1}) | (clause2 - {lit2}), subst
                    )
                    resolvents.append(new_clause)
    return resolvents
```

## 4. Skolemization (Conceptual)
Resolution works on clauses with only universally quantified (implicitly, by convention) free
variables — existential quantifiers must be removed first. **Skolemization** replaces each
existentially quantified variable with a new function (a **Skolem function**) of every universally
quantified variable whose scope contains it (or a **Skolem constant**, if none does).

Worked example: `∀x ∃y Likes(x, y)` ("everyone likes someone") Skolemizes to
`∀x Likes(x, F(x))`, where `F` is a brand-new function symbol — `F(x)` names "the someone that x
likes," whatever object that happens to be, without asserting anything more specific about it.
This is sound for refutation purposes (not a general equivalence-preserving step, a subtlety
left for further study) because it preserves satisfiability, which is exactly what resolution
refutation needs.

## 5. Soundness and Completeness (Result-Level Statement)
FOL resolution, applied to the Skolemized, CNF form of a sentence set, is **sound** (it never
derives a false conclusion from true premises) and **refutation-complete** (if a sentence set is
unsatisfiable, resolution will eventually derive the empty clause) — the FOL analogue of Week 2's
propositional result. Unlike propositional resolution, FOL resolution is not guaranteed to
*terminate* when the sentence set is satisfiable (consistent with the undecidability of FOL in
general); it is complete only in the refutation sense, not as a decision procedure.

## 6. In-Class Exercise
Unify `Knows(john, x)` with `Knows(y, mother(y))` by hand, producing the MGU; then explain, for a
clause `{¬Knows(x, y), Likes(x, y)}` resolving against `{Knows(john, mother(john))}`, which
substitution the resolvent must have applied to it.

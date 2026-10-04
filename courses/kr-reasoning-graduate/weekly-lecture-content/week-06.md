# Week 6 — Lecture Content: Non-Monotonic Reasoning via Answer Set Programming

## 1. From Default Logic to a Computational Non-Monotonic Formalism
The undergraduate course's default logic applies default rules unless their justification is
blocked by a derived fact — non-monotonic, but its extension-based semantics is not tied to a
direct, implementable computation. **Answer Set Programming (ASP)** gives non-monotonic
reasoning a model-theoretic semantics (the **stable model**, Gelfond & Lifschitz, 1988) defined
directly on a logic program, which is exactly what production ASP solvers (e.g., `clingo`)
compute.

## 2. Normal Logic Programs and Negation as Failure
A **normal logic program** is a set of rules:
```
h :- b_1, ..., b_m, not c_1, ..., not c_n.
```
read as "conclude h if every b_i holds and no c_j is known to hold." Crucially, `not c` is
**negation as failure (NAF)**, not classical negation: `not c` holds whenever c cannot be derived
— it is a statement about the program's current conclusions, not about the world. This is exactly
why ASP is non-monotonic: adding a new rule that lets some c_j be derived can retract a
conclusion h that depended on `not c_j`.

## 3. The Stable-Model (Gelfond–Lifschitz) Semantics
Given a candidate set of atoms M (a guess at "the" set of true atoms), form the
**Gelfond–Lifschitz reduct** P^M:
1. Delete every rule whose body contains `not c_i` for some c_i ∈ M (that rule's justification is
   contradicted by the guess M, so it cannot fire).
2. In every surviving rule, delete all remaining `not c_j` literals (c_j ∉ M, so NAF succeeds
   unconditionally under M).

P^M is now a negation-free (definite) program, which has a unique **least model** (computed by
simple forward closure). **M is a stable model of P** exactly when M equals the least model of
P^M — the guess M must be self-consistent with what the reduct it induces actually derives.

**Worked example.** P = { p :- not q.  q :- not p. }.
- Try M = {p}: reduct P^M deletes the second rule (its body `not p` is contradicted since p ∈ M)
  and drops `not q` from the first rule (q ∉ M), leaving { p :- . } whose least model is {p}.
  {p} = {p} — **M = {p} is a stable model.**
- Try M = {q}: by symmetry, **{q} is also a stable model.**
- Try M = {}: reduct keeps both rules unchanged (p, q neither in M), giving { p :- . , q :- . }
  whose least model is {p, q} ≠ {} — **not stable.**
- Try M = {p, q}: reduct deletes both rules (each body's NAF literal is contradicted), leaving the
  empty program, whose least model is {} ≠ {p, q} — **not stable.**
This program has exactly two stable models, {p} and {q} — reflecting a genuine choice the
program leaves open, which a single "the" model (as classical logic would seek) cannot express.

```python
def gl_reduct(rules, M):
    """rules: list of (head, pos_body, neg_body) atom-name tuples. Returns the reduct as a
    negation-free list of (head, pos_body)."""
    reduct = []
    for head, pos, neg in rules:
        if any(c in M for c in neg):
            continue  # rule deleted: its NAF justification is contradicted by M
        reduct.append((head, pos))
    return reduct

def least_model(def_rules):
    """def_rules: list of (head, pos_body) with no negation. Forward closure to a fixpoint."""
    model = set()
    changed = True
    while changed:
        changed = False
        for head, pos in def_rules:
            if all(p in model for p in pos) and head not in model:
                model.add(head)
                changed = True
    return model

def is_stable_model(rules, atoms, M):
    reduct = gl_reduct(rules, M)
    return least_model(reduct) == set(M)

def find_stable_models(rules, atoms):
    from itertools import chain, combinations
    stable = []
    for r in range(len(atoms) + 1):
        for subset in combinations(atoms, r):
            if is_stable_model(rules, atoms, set(subset)):
                stable.append(set(subset))
    return stable
```

## 4. ASP Syntax: Facts, Rules, Constraints
- A **fact** is a rule with an empty body: `edge(a,b).`
- An **integrity constraint** is a rule with an empty head: `:- body.` — it simply *eliminates*
  any candidate stable model whose atoms satisfy `body`, without deriving anything new.

## 5. Worked Problem: Graph Coloring as ASP
In `clingo`-style syntax (shown as real-world context; the Python checker above solves the same
small instances by brute force):
```
color(r). color(g). color(b).
vertex(1). vertex(2). vertex(3).
edge(1,2). edge(2,3).

1 { assign(V,C) : color(C) } 1 :- vertex(V).
:- edge(V,W), assign(V,C), assign(W,C).
```
The choice rule `1 { assign(V,C) : color(C) } 1 :- vertex(V).` says "exactly one color per
vertex" (not expressible as a plain normal-program rule, but reducible for this course's purposes
to enumerating, for each vertex, one NAF-guarded rule per color with mutual-exclusion
constraints — the brute-force checker in Lab 6 encodes this directly as atoms `assign_V_C` with
constraints ruling out two colors for the same vertex and ruling out zero colors for a vertex).
Each stable model of the resulting program corresponds to exactly one valid coloring.

## 6. ASP vs. Default Logic/Circumscription
| | Default logic | Circumscription | ASP (stable models) |
|---|---|---|---|
| Semantics given via | Extensions (apply defaults unless blocked) | Minimizing abnormality predicates | The Gelfond–Lifschitz reduct's least model |
| Computational realization | Not directly tied to one algorithm | Not directly tied to one algorithm | Directly computable (brute force here; industrial solvers in practice) |
| Multiple "answers" | Multiple extensions possible | A minimal-model semantics, can be unique or not | Multiple stable models possible (as in §3's example) — read as alternative solutions |

## 7. In-Class/Lab Exercise
Using `find_stable_models`, confirm the §3 program has exactly the stable models {p} and {q}.
Then encode a 3-vertex, 2-color instance of the graph-coloring problem (one edge forcing two
vertices to differ) as a small rule set with `assign_V_C` atoms and mutual-exclusion/at-least-one
constraints, and use the checker to enumerate all valid colorings.

# Week 3 — Lecture Content: First-Order Logic in Depth

## 1. Why First-Order Logic?
As shown in Week 1, propositional logic needs one sentence per object to express a general rule
("every student who..."), because it has no variables and no way to quantify over a domain.
**First-order logic (FOL)** adds exactly this: objects, relations among them, and quantification.

## 2. Syntax
- **Terms** refer to objects: **constants** (`alice`, `cs101`), **variables** (`x`, `y`), and
  **function applications** (`motherOf(alice)`) — a function returns an object, not a truth value.
- **Atomic sentences** apply a **predicate** to terms: `Completed(alice, cs101)`,
  `Likes(x, motherOf(x))`.
- **Complex sentences** combine atomic sentences with `¬, ∧, ∨, →, ↔`, exactly as in propositional
  logic.
- **Quantifiers**: `∀x φ` ("for all x, φ") and `∃x φ` ("there exists an x such that φ"). A
  variable's **scope** is the sub-sentence the quantifier binds; the same variable name can be
  reused with different scopes (this will matter for correctness in Week 4's unification).

## 3. Semantics: Models
A **model** for FOL consists of a non-empty **domain** of objects and an **interpretation** that
maps every constant to an object, every n-ary predicate to a set of n-tuples of objects (the tuples
for which it holds), and every n-ary function to a mapping from n-tuples of objects to objects.

A sentence is **satisfied** in a model under an assignment of domain objects to its free variables
exactly as you would expect recursively: an atomic sentence is satisfied iff the tuple of
referenced objects is in the predicate's interpretation; `∀x φ` is satisfied iff `φ` is satisfied
for *every* assignment of a domain object to `x`; `∃x φ` iff `φ` is satisfied for *some* such
assignment.

```python
domain = {"alice", "bob", "cs101", "math101"}
completed = {("alice", "cs101"), ("alice", "math101"), ("bob", "cs101")}

def satisfies_forall_exists(domain, relation):
    """Evaluate: for every x in domain, there exists a y in domain such that relation(x, y)."""
    return all(
        any((x, y) in relation for y in domain)
        for x in domain
    )

def satisfies_exists_forall(domain, relation):
    """Evaluate: there exists a y such that for every x, relation(x, y)."""
    return any(
        all((x, y) in relation for x in domain)
        for y in domain
    )

print(satisfies_forall_exists(domain, completed))  # depends on the data above
print(satisfies_exists_forall(domain, completed))  # strictly implied by the above, never implies it
```

Running this on the `completed` relation above: `satisfies_forall_exists` is `False` (e.g.
`cs101` itself has no outgoing `completed` pair, since it is an object, not a student, in this
toy relation), illustrating directly why the two quantifier orders are not interchangeable.

## 4. English-to-FOL Translation
Standard patterns, using `Bird(x)`, `CanFly(x)`:
- "All birds can fly": `∀x (Bird(x) → CanFly(x))` — **not** `∀x (Bird(x) ∧ CanFly(x))`, which
  would incorrectly assert that *everything* in the domain is both a bird and able to fly.
- "Some bird cannot fly": `∃x (Bird(x) ∧ ¬CanFly(x))` — **not** `∃x (Bird(x) → ¬CanFly(x))`,
  which is satisfied by any non-bird and says nothing useful.
- "Every student has taken at least one course": `∀x (Student(x) → ∃c Taken(x, c))`.
- "There is a course every student has taken" (nested, order matters):
  `∃c ∀x (Student(x) → Taken(x, c))`.

## 5. The Quantifier-Scope Trap
`∀x ∃y Likes(x, y)` ("everyone likes someone, possibly a different someone each") is **not**
equivalent to `∃y ∀x Likes(x, y)` ("there is one specific person everyone likes"). The second
*implies* the first but not vice versa. Confirm this with the `satisfies_forall_exists` /
`satisfies_exists_forall` functions above on a relation where each `x` likes a different `y` —
`∀x∃y` holds, `∃y∀x` does not.

## 6. In-Class Exercise
Translate the following into FOL, being careful with implication vs. conjunction and quantifier
order: "No student who has not completed CS101 may enroll in AI301"; "There exists a course that
every instructor has taught." For the second, build a tiny 3-object model by hand where the
sentence is false, and another where it is true.

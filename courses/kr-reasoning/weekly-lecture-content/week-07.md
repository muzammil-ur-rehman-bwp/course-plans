# Week 7 — Lecture Content: Description Logics and Ontologies

## 1. Why Description Logics?
Full FOL (Weeks 3–4) is undecidable: there is no algorithm guaranteed to terminate and correctly
decide every entailment question. **Description logics (DLs)** are a family of carefully
restricted FOL fragments, purpose-built for representing taxonomic/conceptual knowledge, that
trade away some expressiveness in exchange for **decidable** (and often efficient) reasoning —
exactly the expressiveness/inferential-efficiency trade-off from Week 1, resolved deliberately in
favor of efficiency for a well-scoped class of knowledge.

## 2. Basic DL Syntax (the `ALC` Fragment)
- **Concepts** are unary predicates on individuals, e.g. `Person`, `Parent` — written with a
  capital letter, analogous to a class.
- **Roles** are binary predicates relating individuals, e.g. `hasChild`.
- **Constructors** build complex concepts from simpler ones:
  - `C ⊓ D` (conjunction): an individual is in `C ⊓ D` iff it is in both `C` and `D`.
  - `¬C` (negation): an individual is in `¬C` iff it is not in `C`.
  - `∃R.C` (existential restriction): an individual is in `∃R.C` iff it has *some* `R`-related
    individual that is in `C`. (`∃hasChild.Person` = "has at least one child who is a person.")
  - `∀R.C` (universal restriction): an individual is in `∀R.C` iff *every* `R`-related individual
    it has is in `C`. (`∀hasChild.Doctor` = "every child it has is a doctor" — vacuously true if
    it has no children at all.)
- A **TBox** (terminological box) states concept inclusions, e.g. `Parent ⊑ ∃hasChild.Person`
  ("every parent has at least one child who is a person" — read `⊑` as "is a subset of" /
  "is subsumed by").

## 3. The Relationship Between DL and FOL
Every DL concept/role sentence translates directly into a FOL formula with one free variable (by
convention, `x`):

| DL | FOL translation |
|---|---|
| `C ⊓ D` | `C(x) ∧ D(x)` |
| `¬C` | `¬C(x)` |
| `∃R.C` | `∃y (R(x, y) ∧ C(y))` |
| `∀R.C` | `∀y (R(x, y) → C(y))` |
| `Parent ⊑ ∃hasChild.Person` | `∀x (Parent(x) → ∃y (hasChild(x, y) ∧ Person(y)))` |

DLs are exactly those FOL fragments expressible with at most this pattern — one free variable, no
unrestricted quantifier nesting beyond a role restriction. Giving up arbitrary nesting and
arbitrary predicate arity is precisely what buys back decidability.

## 4. Subsumption and a Toy Checker
The central DL reasoning task is **subsumption**: does `C ⊑ D` hold in every model of the TBox
(is every instance of `C` necessarily an instance of `D`)? Real DL reasoners (e.g., Pellet) use a
general **tableau algorithm**; this course instead implements subsumption *semantically*, by
direct model checking over a small finite domain — correct for small, concrete examples, and a
clear illustration of what subsumption means, without building a general-purpose DL reasoner.

```python
def extension(concept, domain, roles, concepts):
    """Compute the set of individuals satisfying a concept expression,
    represented as nested tuples, e.g. ('and', 'Parent', ('exists', 'hasChild', 'Person'))."""
    if isinstance(concept, str):
        return concepts.get(concept, set())
    op = concept[0]
    if op == "and":
        return extension(concept[1], domain, roles, concepts) & extension(concept[2], domain, roles, concepts)
    if op == "not":
        return domain - extension(concept[1], domain, roles, concepts)
    if op == "exists":
        role, filler = concept[1], concept[2]
        filler_ext = extension(filler, domain, roles, concepts)
        return {x for x in domain if any(y in filler_ext for y in roles.get(role, {}).get(x, set()))}
    if op == "forall":
        role, filler = concept[1], concept[2]
        filler_ext = extension(filler, domain, roles, concepts)
        return {x for x in domain if all(y in filler_ext for y in roles.get(role, {}).get(x, set()))}
    raise ValueError(f"unknown constructor: {op}")

def subsumes(d_concept, c_concept, domain, roles, concepts):
    """Check C subseteq D: every individual satisfying C also satisfies D."""
    return extension(c_concept, domain, roles, concepts).issubset(
        extension(d_concept, domain, roles, concepts)
    )
```

### Worked Example
```python
domain = {"alice", "bob", "carol"}
concepts = {"Parent": {"alice", "bob"}, "Person": {"alice", "bob", "carol"}}
roles = {"hasChild": {"alice": {"carol"}, "bob": set()}}

claim = ("exists", "hasChild", "Person")
print(subsumes(claim, "Parent", domain, roles, concepts))  # False: bob is a Parent with no child
```
This correctly returns `False` — `bob` is in `Parent` but has no `hasChild` role filler at all, so
he is *not* in `∃hasChild.Person`, meaning `Parent ⊑ ∃hasChild.Person` does **not** hold for this
particular model (it may still hold as an intended TBox axiom for the real domain; this toy
checker only tests subsumption against one concrete, fully specified model, which is a weaker
guarantee than a true tableau-based decision procedure over all models).

## 5. OWL, RDF, and the Semantic Web
**RDF (Resource Description Framework)** represents web-scale knowledge as triples
(`subject, predicate, object`) — directly analogous to Week 1's `TripleStore`. **OWL (Web
Ontology Language)** is, in essence, a standardized, DL-based ontology language built on top of
RDF: OWL classes correspond to DL concepts, OWL object properties to DL roles, and OWL restriction
axioms to DL constructors like `∃R.C`. The **Semantic Web** vision is to publish such ontologies
and linked RDF data openly on the web so that machines, not just humans, can traverse and reason
over shared, machine-readable knowledge. Production tooling exists for exactly this: **Protégé**
is a widely used ontology editor for building OWL ontologies, and **Pellet** is a DL reasoner that
performs real tableau-based subsumption/consistency checking over them. Neither is required for
this course — they are mentioned so students recognize the real ecosystem this week's toy checker
is a simplified stand-in for.

## 6. In-Class Exercise
Given the TBox axiom `Student ⊑ ∃enrolledIn.Course` and a small 4-individual model (provided),
compute `extension` for `Student` and for `∃enrolledIn.Course` by hand, and determine whether the
axiom holds in that particular model.

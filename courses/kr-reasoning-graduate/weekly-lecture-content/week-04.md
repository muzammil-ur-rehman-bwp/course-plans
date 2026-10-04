# Week 4 — Lecture Content: Description Logics in Depth

## 1. From a Toy Subsumption Check to a Real Decision Procedure
The undergraduate course checked `C ⊑ D` by brute-force model enumeration over a small finite
domain. That does not scale and does not generalize to unrestricted models. The **tableau
algorithm** is the real decision procedure DL reasoners (Pellet, HermiT) use: to test whether a
concept C is satisfiable, try to *build* a model for it by applying completion rules to a
single node labeled with C, branching where necessary, until either every branch closes (clash —
C is unsatisfiable) or some branch is complete and clash-free (that branch describes a model —
C is satisfiable). Subsumption reduces to satisfiability: C ⊑ D iff C ⊓ ¬D is unsatisfiable.

## 2. ALC Syntax Recap
Concepts: atomic concepts (A, B, …), ⊤, ⊥, ¬C, C⊓D, C⊔D, ∃R.C, ∀R.C, for role R.

## 3. The Tableau Completion Rules
A tableau is a tree of nodes, each labeled with a set of concepts it must satisfy, and edges
labeled with roles. Starting from one node x₀ labeled {C} for the concept C under test, apply:

- **⊓-rule.** If x is labeled with C₁⊓C₂ and not already labeled with both, add both C₁ and C₂
  to x's label.
- **⊔-rule.** If x is labeled with C₁⊔C₂ and neither disjunct is already in x's label, **branch**:
  create two alternatives, adding C₁ to x's label in one and C₂ in the other.
- **∃-rule.** If x is labeled with ∃R.C and x has no R-successor labeled with C, create a new node
  y, add the edge x →R y, and label y with {C}.
- **∀-rule.** If x is labeled with ∀R.C and x has an R-successor y, add C to y's label (if not
  already present).

A branch **clashes** (closes) if some node's label contains both an atomic concept A and ¬A (or
contains ⊥). A branch is **complete** if no rule can add anything new to it. The original concept
is **satisfiable** iff at least one branch is complete and clash-free (the clash-free branch's
node labels and role edges literally define a model).

```python
def tableau_satisfiable(concept, trace=None):
    """concept: nested tuple ('and',C1,C2) | ('or',C1,C2) | ('not','A') | ('atom','A')
    | ('exists', role, C) | ('forall', role, C). Returns True/False; a toy ALC tableau
    without role-cycle blocking (sufficient for small, acyclic test concepts)."""
    # One branch = one node's label (a set of concept terms) plus its R-successors (recursively
    # their own sub-tableaux). We track only a single node's label here; ∃/∀ spawn fresh
    # sub-problems for successor nodes.
    def expand(label, successors):
        label = set(label)
        changed = True
        while changed:
            changed = False
            for term in list(label):
                if term[0] == 'and':
                    if term[1] not in label or term[2] not in label:
                        label |= {term[1], term[2]}
                        changed = True
                elif term[0] == 'exists':
                    role, c = term[1], term[2]
                    successors.setdefault(role, []).append({c})
                elif term[0] == 'forall':
                    role, c = term[1], term[2]
                    for succ_label in successors.get(role, []):
                        if c not in succ_label:
                            succ_label.add(c)
            # clash check
            atoms = {t[1] for t in label if t[0] == 'atom'}
            negs = {t[1] for t in label if t[0] == 'not'}
            if atoms & negs:
                return False, label, successors
        # branch on disjunctions
        for term in list(label):
            if term[0] == 'or':
                for choice in (term[1], term[2]):
                    ok, _, _ = expand(label | {choice}, {r: [dict(s) if False else set(s) for s in v]
                                                           for r, v in successors.items()})
                    if ok:
                        return True, label | {choice}, successors
                return False, label, successors
        return True, label, successors

    ok, _, successors = expand({concept}, {})
    if not ok:
        return False
    # recursively check every spawned successor node is itself satisfiable
    for role, labels in successors.items():
        for succ_label in labels:
            # a successor's label is a set of single concepts conjoined
            combined = ('and', *succ_label) if len(succ_label) > 1 else next(iter(succ_label))
            if not tableau_satisfiable(combined):
                return False
    return True
```

## 4. Worked Example
Test C = Person ⊓ ∃hasChild.Doctor ⊓ ∀hasChild.¬Doctor for satisfiability.
- ⊓-rule on the root: label becomes {Person, ∃hasChild.Doctor, ∀hasChild.¬Doctor}.
- ∃-rule: create successor y with label {Doctor} (y is the R-successor witnessing ∃hasChild.Doctor).
- ∀-rule: since the root has the R-successor y, add ¬Doctor to y's label.
- y's label is now {Doctor, ¬Doctor} — **clash**. Every way of building this model fails, so C is
  **unsatisfiable** (correctly: C asserts a child that is both a doctor and, by the universal
  restriction, not a doctor).

If instead C′ = Person ⊓ ∃hasChild.Doctor ⊓ ∀hasChild.Kind, the ∀-rule adds Kind (not ¬Doctor) to
y, giving y label {Doctor, Kind} — no clash, both branches complete — so C′ is **satisfiable**.

## 5. Complexity of DL Reasoning
**ALC concept satisfiability is PSPACE-complete** (Schmidt-Schauß & Smolka, 1991). Intuitively:
the tableau's branching (from ⊔) explores a search tree, but with careful reuse of space across
sibling branches (and a bound on how deep distinct, non-repeating node labels can nest before a
cycle must occur), the whole search can be carried out using only *polynomial* space, even though
it may take exponential time — exactly the PSPACE profile. Adding further constructors or
unrestricted general TBoxes (as in more expressive DLs such as SHIQ, which adds transitive roles,
role hierarchies, inverse roles, and qualified cardinality restrictions) pushes reasoning to
**EXPTIME-complete** — strictly believed harder than PSPACE problems in the standard complexity
hierarchy, though this course states, not re-derives, that relationship.

## 6. OWL 2 Tractable Profiles
Full ALC-and-beyond reasoning is too expensive for some large-scale uses. OWL 2 defines three
profiles that each drop expressiveness for a specific tractability guarantee:

| Profile | Based on | Drops | Guarantee | Typical use |
|---|---|---|---|---|
| **OWL 2 EL** | EL++ | Negation, disjunction | Concept subsumption in polynomial time | Large biomedical ontologies (e.g., SNOMED CT) |
| **OWL 2 QL** | DL-Lite | Most constructors; limited existentials | Conjunctive-query answering rewrites into a relational-DB (SQL) query; AC0/LogSpace data complexity | Querying large relational data through an ontology layer |
| **OWL 2 RL** | Description Logic Programs (pD*) | General existentials/universals on the right | Reasoning implementable by forward-chaining rules, polynomial time | Rule-engine-backed large-scale reasoning |

## 7. In-Class/Lab Exercise
Run `tableau_satisfiable` on both C and C′ from §4 and confirm the results match the hand trace.
Then construct a third concept with a disjunction that forces branching (e.g.,
`('and', ('atom','A'), ('or', ('atom','B'), ('not','A')))`) and trace by hand which disjunct the
algorithm must choose to avoid a clash.

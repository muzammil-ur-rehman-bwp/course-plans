# Week 4 — Lecture Content: Advanced Description Logics — SROIQ

## 1. From ALC to SROIQ
Recall ALC's constructors: ⊓ (conjunction), ⊔ (disjunction), ¬ (negation), ∃R.C (existential
restriction), ∀R.C (value restriction), and the graduate course's tableau completion rules
(⊓-rule, ⊔-rule, ∃-rule, ∀-rule) that search for a clash-free model. **SROIQ** — the description
logic underlying **OWL 2 DL** — keeps all of this and adds exactly the constructs that make OWL 2
DL expressive enough for real ontologies, at a real cost in reasoning complexity.

## 2. New Constructs
- **Role hierarchies (H):** an axiom `R ⊑ S` asserts every pair related by R is also related by
  S (e.g., `hasMother ⊑ hasParent`). Any `∀S.C` on a node must now also propagate along every
  sub-role R ⊑ S, not just S itself.
- **Complex role inclusion axioms (the "R" in SROIQ):** role *chains* such as
  `hasParent ∘ hasParent ⊑ hasGrandparent` — composing two roles and including the composite in a
  third. Unrestricted role inclusion axioms can make satisfiability **undecidable**; SROIQ
  requires the set of role axioms to satisfy a **regularity condition** (informally: the axioms
  can be given a strict partial order under which each chain's roles are not "larger" than the
  included role in a way that creates certain cyclic dependencies) — a real, named restriction
  that is necessary for decidability, stated here as a fact, not re-derived.
- **Number restrictions (N, Q):** `≥n R.C` ("at least n R-successors satisfying C") and
  `≤n R.C` ("at most n"), **qualified** by a concept C (unqualified number restrictions, `≥n R`,
  are the special case C = ⊤).
- **Nominals (O):** a concept `{a}` denotes the singleton set containing exactly the named
  individual a, letting a specific individual appear inside a concept expression (e.g.
  `hasCapital.{paris}` — "has Paris, specifically, as its capital"), which is what lets SROIQ talk
  about *this one named thing* rather than only general class structure.
- **Role characteristics (I):** reflexivity/irreflexivity, symmetry/asymmetry, transitivity, and
  role disjointness as first-class axioms.

## 3. Extending the Tableau
The ⊓/⊔/∃/∀ rules carry over unchanged in spirit. Two new completion rules are needed:
- **≥-rule:** if a node x has `≥n R.C` and does not yet have n pairwise-distinct R-successors all
  satisfying C, create fresh ones (marked pairwise distinct) until it does.
- **≤-rule:** if a node x has `≤n R.C` and has *more than* n R-successors satisfying C, pick two
  of them and **merge** them (identify them as the same node, redirecting all their edges and
  labels onto the merged node) — this is genuinely new bookkeeping ALC's tableau never needed,
  since ALC has no cardinality bound to enforce by *collapsing* successors together.
- **Role-hierarchy propagation:** when applying the ∀-rule for `∀S.C` on x, it must also fire
  along any R-successor where `R ⊑ S`, not only literal S-successors.
- A clash now also includes a node forced `≤n R.C` with more than n *provably pairwise-distinct*
  (nominal-separated) R-successors satisfying C that the merge rule cannot legally identify.

## 4. A Toy Fragment Checker
A full SROIQ tableau is well beyond teaching scale; the following implements the pedagogically
essential new pieces — unqualified number restrictions and role-hierarchy propagation — over a
small, fixed ALC-plus fragment, as a from-scratch demonstration of the key new rules.

```python
from dataclasses import dataclass, field

@dataclass
class Node:
    labels: set          # concept names and ¬concept names asserted true at this node
    successors: dict      # role name -> list of child Node

def role_closure(role, hierarchy):
    """All roles S such that role ⊑* S, including role itself."""
    closure = {role}
    frontier = [role]
    while frontier:
        r = frontier.pop()
        for sub, sup in hierarchy:
            if sub == r and sup not in closure:
                closure.add(sup)
                frontier.append(sup)
    return closure

def propagate_universals(node, universal_restrictions, hierarchy):
    """For each (S, C) in universal_restrictions asserted at node, push C onto every
    successor reachable via a role R with R subsumed by S (role-hierarchy propagation)."""
    changed = False
    for (role, child_list) in node.successors.items():
        implied_by = {s for (r, s) in hierarchy if r == role} | {role}
        for (S, C) in universal_restrictions:
            if S in implied_by:
                for child in child_list:
                    if C not in child.labels:
                        child.labels.add(C)
                        changed = True
    return changed

def check_at_least(node, role, n, concept):
    """≥-rule (unqualified-restriction check): ensure n pairwise-distinct successors via role
    with `concept` present; create fresh ones if short."""
    existing = [c for c in node.successors.get(role, []) if concept in c.labels]
    while len(existing) < n:
        fresh = Node(labels={concept}, successors={})
        node.successors.setdefault(role, []).append(fresh)
        existing.append(fresh)
    return existing

def has_clash(node):
    return any(lbl.startswith("¬") and lbl[1:] in node.labels for lbl in node.labels)
```

## 5. Complexity and Decidability
**ALC concept satisfiability is PSPACE-complete** (graduate course result, recalled here as the
baseline). Adding unrestricted general TBoxes and more constructors escalates this: **SHIQ**
(ALC plus transitive roles, role hierarchies, inverse roles, and qualified number restrictions)
is **EXPTIME-complete**. **SROIQ** goes further still: concept satisfiability in SROIQ is
**N2ExpTime-complete** — a genuine double-exponential jump beyond SHIQ's single exponential,
driven specifically by the interaction of **nominals** with **inverse roles and number
restrictions** (nominals let the tableau "merge" distinguished individuals under constraints that
can force an exponential blow-up in the bookkeeping needed to track them correctly, compounded by
complex role inclusion axioms' own combinatorics). SROIQ remains **decidable**, but only because
of the **regularity condition** on role inclusion axioms stated in §2 — dropping that condition
can make satisfiability checking **undecidable** outright, not merely harder. This is the
accurate, named trade-off: SROIQ buys real expressive power (nominals, qualified cardinalities,
role chains) at the price of double-exponential worst-case reasoning, which is exactly why OWL 2's
tractable profiles (EL, QL, RL — graduate course material) exist as deliberately less expressive,
polynomial-time alternatives for applications that do not need SROIQ's full power.

## 6. In-Class/Lab Exercise
Using `check_at_least` and `propagate_universals`, build a toy node asserting `≥2 hasChild.Happy`
under a role hierarchy `hasChild ⊑ hasRelative` and a universal restriction `∀hasRelative.Known`
on the parent node; run the pipeline and confirm both fresh children end up labeled `Known` (via
hierarchy propagation) in addition to `Happy` (via the ≥-rule), and state in one sentence why this
propagation step has no ALC analogue (ALC has no role hierarchy to propagate along).

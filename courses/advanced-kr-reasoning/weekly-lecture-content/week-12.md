# Week 12 — Lecture Content: Ontology Evolution and Versioning

## 1. Why Deployed Ontologies Are Rarely Static
An ontology, once in production use, accumulates new domain knowledge: classes get added,
properties get refined, axioms get corrected. Any such change can silently alter what the
ontology **entails** — and any downstream application, query, or integrated ontology that relied
on a now-changed entailment can break without warning. This week is a **grounded survey of real
engineering and research problems**, several only partially solved, rather than a single clean
algorithm — this honestly reflects how the field itself treats the topic.

## 2. Change Detection and Diffing
Given two ontology versions O₁ and O₂, naively diffing the *syntactic* axiom sets
(`O₁ \ O₂`, `O₂ \ O₁`) is not the same as identifying the *semantic* change, because a logically
equivalent restatement of an axiom (e.g., rewriting `A ⊑ B ⊓ C` as the two axioms `A ⊑ B` and
`A ⊑ C`) shows up as a large syntactic diff despite changing nothing entailment-wise. A
semantically meaningful diff needs to check entailment-preservation, not just set membership —
itself requiring repeated entailment checks (connecting back to Week 11's machinery) across
however many queries matter to "nothing changed."

## 3. Impact / Consequence Analysis
Distinct from Week 11's justification question ("why does this one entailment hold?"), impact
analysis asks **"what changed?"** — which entailments are gained, and which are lost, between
O₁ and O₂. This matters because an axiom addition is not always purely additive: a new axiom can
interact with existing ones (especially under SROIQ's richer constructs — role chains, number
restrictions, nominals) to produce *unexpected* new entailments the ontology engineer never
intended, or — more dangerously — to make the ontology **inconsistent**, silently invalidating
every entailment at once.

```python
def ontology_diff(axioms_v1, axioms_v2, queries, entailment_checker):
    """axioms_v1/v2: sets of axioms (as hashable objects). queries: list of query atoms.
    entailment_checker: function(frozenset_of_axioms, query) -> bool.
    Returns: syntactic added/removed axioms, and which queries' entailment status changed."""
    added = axioms_v2 - axioms_v1
    removed = axioms_v1 - axioms_v2
    changed_queries = []
    for q in queries:
        held_before = entailment_checker(frozenset(axioms_v1), q)
        held_after = entailment_checker(frozenset(axioms_v2), q)
        if held_before != held_after:
            changed_queries.append((q, held_before, held_after))
    return {"added": added, "removed": removed, "changed_queries": changed_queries}
```

## 4. Backward Compatibility and Versioning Policy
Should a new ontology version be required to still entail **everything** the old version did (a
strict **backward-compatible extension**, only ever adding new entailments), or is some
entailment *loss* an accepted, intentional correction (fixing a modeling mistake necessarily
removes whatever that mistake used to, wrongly, entail)? Both are legitimate versioning policies
for different situations, but a dependent system needs to be told **which kind of change
occurred** — silently shipping a "fix" that happens to break a downstream system's assumptions,
with no signal that this was an intentional, non-backward-compatible correction rather than an
accidental regression, is the real-world failure mode this distinction exists to prevent.

## 5. Modularity as a Partial Mitigation
Structuring a large ontology into smaller, more independently evolvable **modules** (each
covering a sub-domain, with explicit, deliberately minimal interfaces between modules) can make a
local change's effect more predictable and more bounded — a change confined to one module's
internals, not touching its interface, should not affect reasoning in other modules. This is only
a **partial** mitigation: module boundaries can still interact in non-obvious ways (an axiom in
one module can combine with an axiom in another, through a shared term, to produce an entailment
neither module's engineer anticipated), and determining a safe, complete modularization for an
arbitrary existing ontology is itself a hard, only partially automatable engineering problem.

## 6. In-Class/Lab Exercise
Given two small, hand-written toy ontology versions (5–8 axioms each, one version a deliberate
"fix" of a modeling mistake in the other) and a fixed list of 4–5 queries, run `ontology_diff`
with a simple Horn-clause entailment checker (reuse Week 6/11's `least_model`-based check).
Identify: (a) which queries' entailment status changed; (b) whether the change should be
considered backward-compatible, citing specifically which entailments were preserved and which
were lost; (c) write one sentence on what a dependent system relying on one of the now-lost
entailments would need to be told, and when, to avoid silent breakage.

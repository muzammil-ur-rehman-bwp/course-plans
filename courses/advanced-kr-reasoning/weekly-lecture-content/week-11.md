# Week 11 — Lecture Content: Explanation and Justification Research

## 1. Why "Why Does My Ontology Entail This?" Is a Genuine Research Problem
In an expressive DL such as SROIQ, a single entailment can follow from a large, non-obvious
combination of axioms interacting through role hierarchies, number restrictions, and nominals
(Week 4). Simply re-running the tableau algorithm and printing its trace does not, by itself,
give a human-usable explanation: the trace records *every* rule application the algorithm
happened to perform, most of which may be irrelevant bookkeeping rather than the actual reason
the entailment holds, and a single run of the algorithm finds only *one* clash/derivation even
when the entailment genuinely has multiple, independent underlying reasons.

## 2. Justifications
Given an ontology O and an entailment α (O ⊨ α), a **justification** for α is a **minimal** subset
J ⊆ O such that J ⊨ α — minimal in the sense that no strict subset of J entails α. Minimality
matters because O itself trivially entails α (if O ⊨ α, certainly the whole of O does); an
explanation earns its usefulness only to the extent it isolates the smallest set of axioms that
actually suffices. A single entailment can have **several distinct justifications** — genuinely
different reasons, each independently sufficient, that the entailment holds; reporting only one
can mislead a user into thinking it is the *only* reason, when removing those specific axioms
would leave the entailment intact via a different route.

```python
from itertools import combinations

def entails(axiom_subset, query, entailment_checker):
    """entailment_checker: function(frozenset_of_axioms, query) -> bool."""
    return entailment_checker(frozenset(axiom_subset), query)

def all_justifications(ontology, query, entailment_checker):
    """Black-box justification finder: tests subsets by increasing size, keeping only
    minimal entailing subsets (i.e. skip any subset that is a strict superset of an
    already-found justification)."""
    ontology = list(ontology)
    found = []
    for size in range(1, len(ontology) + 1):
        for subset in combinations(ontology, size):
            subset_set = frozenset(subset)
            if any(j <= subset_set for j in found):
                continue  # not minimal: a found justification is already a subset of this
            if entails(subset_set, query, entailment_checker):
                found.append(subset_set)
    return found
```

## 3. Black-Box vs. Glass-Box Justification Finding
**Black-box** methods (as coded above) repeatedly call an existing, unmodified DL reasoner as a
sub-procedure, testing candidate axiom subsets for entailment without looking inside the
reasoning algorithm at all — simple to implement against any reasoner, but naively exponential:
in the worst case, checking every subset of a size-n ontology requires up to 2^n entailment
tests. **Glass-box** methods instead instrument the tableau algorithm itself to track, during a
single run, which axioms were actually *used* on the branch that produced a clash or derivation —
potentially far more efficient (one run can surface a justification directly, without separately
re-testing subsets) but requires reasoner-internal modification unavailable for an off-the-shelf
black-box reasoner. Real production systems (e.g., justification-finding features in tools built
on Pellet/HermiT/FaCT++) typically combine both: glass-box hints narrow the search, and black-box
minimality checks confirm the result. Scaling either approach to realistically large ontologies
while finding *all* justifications (not just one) remains active, open research.

## 4. Proof Trees Revisited
The graduate course's **proof tree** (a derived conclusion's justification as a recursive rule-
application structure, down to leaf facts) is the natural notion for rule-based, forward-chaining
derivation, where typically one wants *a* human-readable derivation. Once a logic allows multiple
*independent* derivations of the same conclusion — as expressive DL entailment generally does — "a
proof tree" and "the set of all justifications" become genuinely different explanation notions: a
single proof tree answers "here is one way this follows," while the justification set answers
"here are all the minimally sufficient reasons this follows, and nothing less would do for any of
them." Which one is the *right* deliverable is itself a live, debated design question in the
explanation-for-ontologies literature — a single smallest justification is often most readable,
but reporting only it can hide genuinely independent support an engineer auditing the ontology
would want to know about (e.g., if one justification depends on an axiom under revision, knowing
whether a *second*, independent justification would keep the entailment alive after that axiom is
removed is exactly the information a single proof tree, or a single justification, cannot supply).

## 5. In-Class/Lab Exercise
Hand-craft a toy ontology (5–7 simple propositional-Horn-style "axioms," treated as a toy stand-in
for DL axioms for this exercise) with a query entailed by two genuinely disjoint minimal subsets.
Run `all_justifications` with a simple Horn-clause entailment checker (reuse Week 6's
`least_model` function as the entailment test: `query in least_model(axiom_subset_as_facts_and_rules)`)
and confirm it reports both justifications and no non-minimal supersets. Write a short paragraph
stating, for this example, which of "a single proof tree" or "the full justification set" would
better serve an ontology engineer deciding whether a proposed axiom removal is safe, and why.

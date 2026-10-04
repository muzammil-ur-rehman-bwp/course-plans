# Week 8 — Lecture Content: Neuro-Symbolic Integration II; Midterm Review

## 1. Neural Theorem Proving (Conceptual Survey)
Classical backward-chaining proof search (e.g., SLD resolution, or the graduate course's
resolution/tableau methods) relies on **exact unification**: two atoms either unify (with some
substitution) or they do not, and proof search branches only on exact syntactic/semantic matches.
**Neural theorem proving** (the line of work following the original Neural Theorem Prover
architecture) replaces this binary test with a **soft, differentiable unification score**: every
symbol is embedded as a vector, and two atoms "unify" to a *degree* given by the similarity (e.g.,
a learned or cosine similarity) of their embeddings, rather than exactly or not at all. Proof
search then becomes a search over a continuous space of partial unification scores, with the
overall proof's score some aggregation (e.g., a soft minimum, echoing Week 7's t-norms) of the
individual unification steps involved.

**What is gained:** the system can "prove" a query using a rule whose literal symbols do not
exactly match any fact in the knowledge base, provided an embedded symbol is *close enough* to one
that does — a form of learned generalization a purely symbolic prover cannot offer (it either
unifies or does not, with no notion of "almost"). **What is lost:** exactness and interpretability
— a soft proof score is not a guarantee of logical validity, and the search space is far larger
and noisier than exact symbolic search (every pair of symbols has *some* similarity, so pruning
branches that would be trivially rejected by exact unification becomes much harder). This remains
a genuinely open research question: how to get the generalization benefit without losing a
meaningful soundness guarantee.

## 2. Combining KG Embeddings with Explicit Logical Constraints
The graduate course's TransE model proposes candidate facts via link prediction, ranked by score
alone, with no guarantee the top-ranked candidates are *logically consistent* with anything
already known. A grounded, non-hype neuro-symbolic pattern: use embedding-based ranking to
**propose** candidates, then use explicit (hard or Week 7's soft) logical constraints to
**filter, re-rank, or veto** candidates that violate known structure.

```python
def filter_candidates(candidates, hard_constraints):
    """candidates: list of (head, relation, tail, score) sorted by score descending.
    hard_constraints: list of functions (h, r, t) -> bool, True = constraint satisfied.
    Returns only candidates passing every constraint."""
    survivors = []
    for (h, r, t, score) in candidates:
        if all(c(h, r, t) for c in hard_constraints):
            survivors.append((h, r, t, score))
    return survivors

def no_self_loop(relation_name):
    def constraint(h, r, t):
        return not (r == relation_name and h == t)
    return constraint

def type_constraint(relation_name, head_type_set, tail_type_set, entity_types):
    def constraint(h, r, t):
        if r != relation_name:
            return True
        return entity_types.get(h) in head_type_set and entity_types.get(t) in tail_type_set
    return constraint

# Example: veto any TransE-proposed "parentOf" triple where head == tail (self-parenthood),
# and require both endpoints to be typed "Person".
entity_types = {"alice": "Person", "bob": "Person", "acme": "Organization"}
constraints = [
    no_self_loop("parentOf"),
    type_constraint("parentOf", {"Person"}, {"Person"}, entity_types),
]
candidates = [("alice", "parentOf", "bob", 0.91), ("alice", "parentOf", "alice", 0.85),
              ("alice", "parentOf", "acme", 0.60)]
print(filter_candidates(candidates, constraints))
# only ("alice", "parentOf", "bob", 0.91) survives
```
This is the same spirit as the graduate course's Week 11 rule-mining-plus-embedding survey, now
seen through this week's constraint-filtering lens rather than independent rule mining: rules give
exact, explainable coverage where they apply but are brittle to noise and incompleteness;
embeddings generalize smoothly and tolerate noise but give no guarantee and can be wrong with
high confidence — combining them trades off these complementary strengths and weaknesses, without
fully resolving either side's limitation.

## 3. Honest Assessment
Neither neural theorem proving nor embedding-plus-constraint hybrids is a solved problem. Both
are active research directions precisely because the central tension — generalization/noise-
tolerance versus exactness/explainability — has no known general resolution; this week's
material should be read as a snapshot of where the research currently stands, not a settled
engineering recipe.

## 4. Midterm Review (Weeks 1–8)
Review checklist, one line each: (1) HOL/type theory — why FOL cannot quantify over predicates,
simply-typed lambda calculus, β-reduction; (2) many-valued/paraconsistent logics — K3 vs. Ł3
implication, FDE's non-explosive B value; (3) SROIQ — role hierarchies, complex role inclusions,
number restrictions, nominals, N2ExpTime-completeness; (4) ASPIC+ — rebutting vs. undercutting
attacks, ASPIC+ as a Dung-framework generator; (5) distribution semantics — total choices,
independent-fact mixture, contrast with MLN log-linear weighting; (6) differentiable/fuzzy logic —
t-norms/t-conorms, why product beats Gödel for gradient-based learning; (7) neural theorem proving
and embedding-constraint hybrids — what is gained/lost by soft unification and constraint
filtering.

## 5. In-Class/Lab Exercise
Using `filter_candidates`, construct a second type constraint that would veto a candidate your
Week 8 TransE-style ranking (reused from the graduate course's lab) actually proposed near the top
of its ranking, and report whether the embedding model "confidently" proposed a fact that a single
hard constraint immediately rules out — connect this directly to §3's honest-assessment point
about embeddings being wrong with high confidence.

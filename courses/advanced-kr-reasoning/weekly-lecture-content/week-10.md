# Week 10 — Lecture Content: Multi-Agent Belief Merging

## 1. From Single-Agent Revision to Multi-Agent Merging
The graduate course's AGM framework has one privileged belief set K receiving one new, trusted
input φ. **Belief merging** faces a structurally different problem: several agents, each with
their own consistent belief set K₁,...,Kₙ, must be combined into a single result — and critically,
in the base case **no agent's belief set is privileged over any other's**. The agents are peers;
the operator must *adjudicate*, not simply *incorporate*.

## 2. Distance-Based Merging Operators
Generalize the graduate course's **Dalal revision** (which selects the models of the new
sentence φ closest, by Hamming distance, to the models of K) to a **profile** of belief sets
K₁,...,Kₙ plus a set of hard **integrity constraints** IC the merged result must satisfy. Represent
each Kᵢ semantically by `Mod(Kᵢ)`, its set of satisfying valuations. For a candidate valuation v,
define its **distance to the profile** as an aggregate (e.g., sum or max) of its distance to each
Kᵢ's closest model:
```
d(v, Ki) = min_{w ∈ Mod(Ki)} Hamming(v, w)
d(v, profile) = Σ_i d(v, Ki)          (the "Σ" aggregation; "max" is the other standard choice)
```
The merged result's models are exactly the valuations in `Mod(IC)` that **minimize**
`d(v, profile)`:
```
Mod(Δ(K1,...,Kn ; IC)) = argmin_{v ∈ Mod(IC)} d(v, profile)
```

```python
from itertools import product

def hamming(v1, v2):
    return sum(a != b for a, b in zip(v1, v2))

def all_valuations(n_vars):
    return list(product([False, True], repeat=n_vars))

def models_of(formula_fn, valuations):
    return [v for v in valuations if formula_fn(v)]

def distance_to_set(v, model_set):
    return min(hamming(v, w) for w in model_set) if model_set else float("inf")

def merge(profiles, ic_fn, n_vars, aggregate="sum"):
    """profiles: list of model-sets (one per agent, each a list of valuations).
    ic_fn: function(valuation) -> bool, the integrity constraint.
    Returns the merged result's models (the minimizers) and their shared distance."""
    valuations = all_valuations(n_vars)
    ic_models = [v for v in valuations if ic_fn(v)]
    best_score, best_models = None, []
    for v in ic_models:
        dists = [distance_to_set(v, profile) for profile in profiles]
        score = sum(dists) if aggregate == "sum" else max(dists)
        if best_score is None or score < best_score:
            best_score, best_models = score, [v]
        elif score == best_score:
            best_models.append(v)
    return best_models, best_score
```

## 3. The Merging Postulates
A rational merging operator Δ is standardly expected to satisfy postulates generalizing AGM's
spirit to profiles (in the manner of the Konieczny–Pino Pérez postulates), including:
- **(IC0)** `Δ(profile; IC) ⊨ IC` — the merged result always respects the integrity constraints.
- **(IC1)** if the profile is jointly consistent with IC (i.e. `⋂ᵢ Kᵢ ∧ IC` is satisfiable),
  merging is equivalent to simple conjunction — merging only has real work to do when the sources
  actually conflict.
- **(IC2)** `Δ` is **commutative** in the agents — reordering the profile does not change the
  result; no agent is privileged by its position in the input list.
- **(IC3)** logical-equivalence invariance — merging gives logically equivalent results when every
  Kᵢ is replaced by a logically equivalent belief set.

The code above satisfies IC2 by construction (the aggregate distance sums/maxes over the profile
symmetrically) and satisfies IC0 by construction (only IC-models are ever considered).

## 4. Merging vs. Revision: The Key Structural Difference
AGM revision: *one* belief set K, *one* new, trusted sentence φ, asking "how should K change to
accept φ, giving up as little as possible?" Belief merging: *several* belief sets of equal
standing, asking "what does the group, as a whole, now jointly believe, given that its members
disagree?" A revision scenario ("we learn the patient's test result was mistaken" — one trusted
correction to one existing belief set) and a merging scenario ("three independent diagnosticians
disagree about the patient's condition, none more authoritative than the others") can look
superficially similar (both involve combining potentially-conflicting propositional information)
but call for genuinely different operators — revision privileges the *new* input over the *old*
belief set; merging privileges *no* input over any other.

## 5. In-Class/Lab Exercise
Encode three agents' beliefs over three propositional variables `(p, q, r)` as small formulas with
some pairwise disagreement (e.g., agent 1 believes `p ∧ ¬q`, agent 2 believes `¬p ∧ q`, agent 3
believes `r`), under the integrity constraint `r` (hard-required). Using `merge`, compute the
merged result's models under the sum aggregation and again under the max aggregation, and report
whether the two aggregations agree — if they disagree, identify which valuation the sum
aggregation prefers that the max aggregation does not, and explain in one sentence what each
aggregation choice says about how "fair" the merge is to the agent whose beliefs are most
violated in the result.

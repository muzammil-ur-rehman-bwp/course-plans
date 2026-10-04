# Week 6 — Lecture Content: Probabilistic Logic Programming

## 1. The Distribution Semantics
A probabilistic logic program (in the style of ProbLog) consists of **probabilistic facts**,
each written `p :: fact` (fact holds with probability p, independently of every other
probabilistic fact), plus ordinary definite-clause rules built over them. A **total choice** θ is
a selection, for every probabilistic fact, of whether it is included or excluded. Each total
choice induces a unique ordinary (non-probabilistic) logic program P_θ, which — being a plain
set of definite clauses with no negation — has a unique **least model** LM(P_θ), computed by
simple fixpoint iteration exactly as in the graduate course's ASP groundwork (minus any `not`).
The probability of a total choice is the product of p for every included fact and (1−p) for every
excluded one:
```
P(θ) = Π_{f included} p_f  ·  Π_{f excluded} (1 - p_f)
```
The probability of a query atom q is the sum, over every total choice whose induced least model
entails q, of that choice's probability:
```
P(q) = Σ_{θ : q ∈ LM(P_θ)} P(θ)
```

## 2. Contrast with Markov Logic Networks
This is formally a **discrete mixture over possible worlds**, structurally different from the
graduate course's MLN log-linear distribution. An MLN assigns a world's unnormalized probability
from **weighted formula-satisfaction counts**, `exp(Σ_i w_i n_i(x))`, and requires a partition
function Z summing over all worlds to normalize. The distribution semantics instead assigns a
world's probability **directly** as a product of independent fact-level probabilities — no
partition function is needed, because the probabilities of the *facts* are already normalized
(p and 1−p sum to 1 for each one independently), and rules are **hard logical consequences**
derived from a chosen total choice, not weighted soft constraints that can be violated at a
probabilistic cost. Both formalisms define a distribution over "which atoms are true," but an MLN
reasons from *weighted preferences among worlds*, while the distribution semantics reasons from
*independent coin flips on facts plus deterministic logical consequence*.

## 3. A Brute-Force Evaluator
```python
from itertools import product as iproduct

def least_model(facts, rules):
    """facts: set of true atoms. rules: list of (head, [body_atoms]). Fixpoint least model."""
    model = set(facts)
    changed = True
    while changed:
        changed = False
        for head, body in rules:
            if head not in model and all(b in model for b in body):
                model.add(head)
                changed = True
    return model

def distribution_semantics(prob_facts, rules, query):
    """prob_facts: dict atom -> probability. rules: list of (head, [body_atoms]).
    Returns P(query) by brute-force enumeration of all 2^k total choices."""
    names = list(prob_facts)
    total = 0.0
    for choice_bits in iproduct([0, 1], repeat=len(names)):
        included = {n for n, b in zip(names, choice_bits) if b == 1}
        prob = 1.0
        for n, b in zip(names, choice_bits):
            prob *= prob_facts[n] if b == 1 else (1 - prob_facts[n])
        model = least_model(included, rules)
        if query in model:
            total += prob
    return total

# Example: 0.6::rains, 0.3::sprinkler_on, wet :- rains. wet :- sprinkler_on.
prob_facts = {"rains": 0.6, "sprinkler_on": 0.3}
rules = [("wet", ["rains"]), ("wet", ["sprinkler_on"])]
print(distribution_semantics(prob_facts, rules, "wet"))
# = 1 - P(no rains)*P(no sprinkler) = 1 - 0.4*0.7 = 0.72
```
By hand: the four total choices give `{rains,sprinkler}` (p=0.18, wet=True),
`{rains}` (p=0.42, wet=True), `{sprinkler}` (p=0.12, wet=True), `{}` (p=0.28, wet=False). Summing
the probabilities of the wet=True choices: 0.18+0.42+0.12 = 0.72, matching
`1 - (1-0.6)(1-0.3) = 0.72` exactly, as expected for two independent causes of a disjunctively
caused effect.

## 4. Inference at Scale (Conceptual)
Brute-force enumeration over all 2^k total choices is exponential in the number of probabilistic
facts k — fine for teaching-scale k, hopeless for realistic k. Production systems such as ProbLog
instead compile the Boolean formula describing "which total choices entail q" into a **Binary
Decision Diagram (BDD)**, a compact data structure that exploits shared sub-structure across many
total choices so the query's probability can be computed by a single weighted traversal of the
BDD rather than by enumerating every choice separately — this can turn an exponential-in-k
computation into one that is manageable for realistic programs whenever the underlying Boolean
formula compiles to a BDD of reasonable size (itself not always guaranteed, since BDD size depends
on variable ordering and the formula's structure). This is described here conceptually as real
production machinery; it is not implemented in this course.

## 5. In-Class/Lab Exercise
Extend the example program with a third probabilistic fact `0.5 :: broken_sprinkler` and a rule
`wet :- sprinkler_on, not_broken` style interaction is not directly expressible without negation
—instead add `dry_lawn :- broken_sprinkler, sprinkler_on` as a second query and compute
`P(dry_lawn)` by hand via total-choice enumeration, then verify with `distribution_semantics`.
Write two sentences contrasting this program's independent-fact view with how the same scenario
would be encoded as a weighted-formula MLN (what would the weighted formulas be, and where would
a partition function enter that this week's semantics does not need).

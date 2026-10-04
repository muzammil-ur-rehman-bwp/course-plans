# Week 9 — Lecture Content: Markov Logic Networks in Depth

*(Midterm Exam occupies the first part of this week's session; this content covers the
second-half lecture.)*

## 1. Beyond the Undergraduate Survey
The undergraduate course mentioned Markov Logic Networks (MLNs) briefly, as one of several
uncertainty formalisms beyond Bayes. This week gives MLNs their actual formal treatment:
precise grounding, the log-linear distribution, and what inference over that distribution means.

## 2. The MLN Formalism
A Markov Logic Network L (Richardson & Domingos, 2006) is a finite set of pairs
**(F_i, w_i)**: a first-order formula F_i and a real-valued **weight** w_i. Given a finite set of
constants (a domain), **grounding** L produces an ordinary (propositional) Markov network:
- One binary random variable per **ground atom** (every way of substituting domain constants for
  F_i's free variables, applied to every predicate).
- One **feature** per **grounding** of each formula F_i — i.e., one feature for every way of
  substituting constants for F_i's variables, each feature's value being 1 if that particular
  grounding of F_i is satisfied by the current truth assignment, 0 otherwise.

## 3. The Log-Linear Distribution
Let x be a **possible world**: a truth assignment to every ground atom. Let n_i(x) be the number
of groundings of F_i that are satisfied in x. The MLN defines:
```
P(x) = (1/Z) · exp( Σ_i w_i · n_i(x) )
Z = Σ_{x'} exp( Σ_i w_i · n_i(x') )     (sum over every possible world x')
```
**Reading the formula.** A world that satisfies *more* groundings of a *positively*-weighted
formula gets an exponentially larger unnormalized score — more probable, but never required to
satisfy every grounding, since a world violating some groundings is merely *less* probable, not
impossible (unlike classical FOL, where a single violated universally-quantified formula makes a
world inconsistent). As w_i → ∞ for every formula, the distribution concentrates all its mass on
worlds satisfying every grounding of every infinite-weight formula — classical FOL is recovered
as the limiting case of "infinitely confident" weights.

```python
from itertools import product

def ground_atoms(predicates, constants):
    """predicates: {name: arity}. Returns list of ground atom names, e.g. 'Smokes_Alice'."""
    atoms = []
    for name, arity in predicates.items():
        for combo in product(constants, repeat=arity):
            atoms.append(f"{name}_" + "_".join(combo))
    return atoms

def count_satisfied_groundings(formula_template, constants, world):
    """formula_template: function(*consts) -> bool, evaluated against `world` (dict atom->bool)
    via closures baked into the template. Returns n_i(x) for one formula over all groundings."""
    n_vars = formula_template.__code__.co_argcount
    count = 0
    for combo in product(constants, repeat=n_vars):
        if formula_template(*combo)(world):
            count += 1
    return count

def mln_distribution(formulas_weights, constants, predicates):
    """formulas_weights: list of (formula_template, weight). Returns {world_tuple: probability}
    enumerating every possible world over the grounded atoms (toy-scale domains only)."""
    atoms = ground_atoms(predicates, constants)
    worlds = list(product([False, True], repeat=len(atoms)))
    scores = []
    for bits in worlds:
        world = dict(zip(atoms, bits))
        score = sum(w * count_satisfied_groundings(f, constants, world)
                    for f, w in formulas_weights)
        scores.append(score)
    import math
    exp_scores = [math.exp(s) for s in scores]
    Z = sum(exp_scores)
    return {worlds[i]: exp_scores[i] / Z for i in range(len(worlds))}
```

## 4. Worked Grounding Example
Domain: constants = {Alice, Bob}. Predicates: Smokes/1, Friends/2. Formulas:
- F1 = "Smokes(x) → Smokes(y) whenever Friends(x,y)" i.e. ∀x,y. Friends(x,y) → (Smokes(x) →
  Smokes(y)), weight w1 = 1.1 (a soft "friends have correlated smoking habits" tendency).
- F2 = "¬Smokes(x)" i.e. ∀x. ¬Smokes(x), weight w2 = 0.2 (a weak prior preference for
  non-smoking).

Grounding F1 over {Alice, Bob} gives 4 groundings (x,y ∈ {Alice,Bob}²); grounding F2 gives 2
groundings (x ∈ {Alice,Bob}). Ground atoms: Smokes_Alice, Smokes_Bob, Friends_Alice_Alice,
Friends_Alice_Bob, Friends_Bob_Alice, Friends_Bob_Bob (6 ground atoms, 2^6 = 64 possible worlds
at this toy scale — exactly the brute-force-enumerable size `mln_distribution` targets).

**Interpretation, not full computation here:** a world where Alice and Bob are Friends and both
smoke satisfies all 4 groundings of F1 that involve them in a "friends→correlated" pattern (since
there is no case where Friends holds but one smokes and the other does not) and violates 2
groundings of F2 (both ¬Smokes(Alice), ¬Smokes(Bob) fail) — its score trades off w1's full credit
for consistency against w2's two violations. A world where they are Friends but only one smokes
violates one grounding of F1 (the implication fails exactly where Friends holds and the
antecedent Smokes(x) is true but Smokes(y) is false) and satisfies one grounding of F2 — the
log-linear model directly compares these worlds' relative plausibility via their differing scores
rather than ruling either one out categorically.

## 5. Inference, Conceptually
Even this toy 6-atom domain already requires enumerating 64 worlds; real MLN domains are far too
large to enumerate. Two standard conceptual approaches (not implemented at scale in this course):
**MAP inference** (find the single most probable world — equivalent to a weighted satisfiability
problem, maximizing Σw_i n_i(x)) and **MCMC-based approximate marginal inference** (e.g., Gibbs
sampling over the ground Markov network to estimate P(query | evidence) without ever summing over
all of Z). Lab 9's brute-force evaluator is explicitly a teaching-scale substitute for both.

## 6. In-Class/Lab Exercise
Using `mln_distribution` on a 2-constant, single-formula toy MLN (e.g., just F2 above), compute
the full distribution, confirm the all-non-smoking world has the highest probability, then raise
w2 to a large value (e.g., 20) and confirm the distribution concentrates almost entirely on that
one world — a concrete demonstration of the hard-constraint limiting case from §3.

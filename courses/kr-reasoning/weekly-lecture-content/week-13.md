# Week 13 — Lecture Content: Reasoning with Uncertainty Beyond Bayes

## 1. Why Look Beyond Bayesian Networks?
Pure Bayesian networks (Week 12) represent probability well but struggle to encode general
relational/logical structure with exceptions compactly (every relationship needs its own node and
CPT). Pure FOL (Weeks 3–4) represents relational structure well but has no notion of degree of
belief — a sentence is simply true, false, or (classically) not yet determined. Two formalisms
that address these gaps from different directions are surveyed this week, at a conceptual level.

## 2. Markov Logic Networks (Conceptual Survey)
A **Markov logic network (MLN)** is a set of first-order formulas, each with an attached
**weight**. Unlike classical FOL, where a formula is either satisfied or the KB is inconsistent,
an MLN formula is a *soft* constraint: a possible world (a full truth assignment to all ground
atoms) is not required to satisfy every formula, but worlds that satisfy *more* (weighted)
formula instances are assigned higher probability than worlds that satisfy fewer. Formally (given
here for context, not for direct implementation), the probability of a world `x` is proportional
to `exp(Σ_i w_i · n_i(x))`, where `n_i(x)` counts how many groundings of formula `i` are satisfied
in `x` and `w_i` is that formula's weight — a higher weight means violating that formula is more
heavily penalized, but never strictly forbidden (a weight of `+∞` would recover a hard, classical
FOL constraint).

### A Small Worked Comparison (By Hand, Not Implemented)
Two weighted formulas: `Smokes(x) → Cancer(x)` with weight `1.5`, and
`Friends(x, y) ∧ Smokes(x) → Smokes(y)` with weight `1.1`. Consider two candidate worlds for a
tiny domain `{anna, bob}` where `Friends(anna, bob)` holds and `Smokes(anna)` holds: a world where
`Smokes(bob)` also holds satisfies one more grounding of the second formula than a world where it
does not, so (all else equal) it is assigned a *higher* probability, but a world with
`¬Smokes(bob)` is still possible, just less likely — this is the essential behavior that
distinguishes an MLN from classical FOL, where `Friends(anna,bob) ∧ Smokes(anna)` would need a
*hard* rule to force `Smokes(bob)` and no alternative would be permitted at all. This course
introduces MLNs at exactly this conceptual level; implementing MLN inference (which generally
requires sampling or specialized algorithms) is beyond this week's lab.

## 3. Fuzzy Logic Basics
**Fuzzy logic** generalizes classical set membership (`0` or `1`) to a **degree of membership**
in `[0, 1]`. A **fuzzy set** is defined by a **membership function** `μ_A(x)` giving the degree to
which `x` belongs to fuzzy set `A`. A common shape is the **triangular membership function**:

```python
def triangular_membership(x, a, b, c):
    """Degree of membership in a triangular fuzzy set with left foot a, peak b, right foot c."""
    if x <= a or x >= c:
        return 0.0
    if x == b:
        return 1.0
    if x < b:
        return (x - a) / (b - a)
    return (c - x) / (c - b)

# "Warm" temperature: rises from 15, peaks at 22, falls to 29.
print(triangular_membership(20, 15, 22, 29))  # partial membership, between 0 and 1
```

**Fuzzy set operations** generalize Boolean operations:

```python
def fuzzy_and(a, b):
    return min(a, b)

def fuzzy_or(a, b):
    return max(a, b)

def fuzzy_not(a):
    return 1 - a
```

## 4. A Worked Fuzzy Rule Example
Toy controller: "IF temperature is Warm AND humidity is High THEN fan speed is Medium." Given
membership degrees `μ_Warm(20) = 0.71` and `μ_High(60) = 0.6`, the rule's firing strength is
`fuzzy_and(0.71, 0.6) = 0.6` (the minimum) — this firing strength then scales how strongly the
"Medium" fan-speed fuzzy set contributes to the final output (a full controller combines several
such rules and **defuzzifies** the combined result into one crisp output value; defuzzification
itself is mentioned here as the next conceptual step but not required in this course's lab).

```python
def evaluate_rule(temp_value, humidity_value):
    warm_degree = triangular_membership(temp_value, 15, 22, 29)
    high_degree = triangular_membership(humidity_value, 40, 70, 100)
    return fuzzy_and(warm_degree, high_degree)  # the rule's firing strength

print(evaluate_rule(20, 60))
```

## 5. In-Class Exercise
For `μ_Cold(x)` a triangular function with feet `(0, 10, 20)` and `μ_Hot(x)` with feet
`(20, 30, 40)`, compute both memberships for `x = 18` by hand, then compute `fuzzy_or` of the two
— discuss what it means for a value to have nonzero membership in *both* "Cold" and "Hot" at once,
something impossible in classical (crisp) set membership.

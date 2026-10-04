# Week 5 — Lecture Content: Automated Theorem Proving in Depth

## 1. From One Tableau to a General Toolkit
Week 4's ALC tableau was a decision procedure for one restricted fragment. This week generalizes
in two directions: the **sequent calculus** (a different, proof-rule-based style of derivation)
and the **first-order tableau** (the same branching idea as Week 4, extended to unrestricted
FOL), and then sharpens the undergraduate course's plain resolution with **refinement
strategies** that prune its search space while staying sound and complete.

## 2. The Sequent Calculus
A **sequent** Γ ⊢ Δ (Γ, Δ finite sets of formulas) asserts: the conjunction of Γ entails the
disjunction of Δ. A derivation is a tree of sequents built downward from axiom sequents φ ⊢ φ by
**rules**, each either introducing a connective on the right (building toward Δ) or eliminating
it on the left (decomposing an assumption in Γ):

- **∧-right**: from Γ⊢Δ,φ and Γ⊢Δ,ψ, infer Γ⊢Δ,φ∧ψ.
- **∧-left**: from Γ,φ,ψ⊢Δ, infer Γ,φ∧ψ⊢Δ.
- **→-right**: from Γ,φ⊢Δ,ψ, infer Γ⊢Δ,φ→ψ.
- **∀-right**: from Γ⊢Δ,φ[x/c] where c is a **fresh constant not occurring elsewhere in the
  sequent** (the *eigenvariable condition* — c must stand for an arbitrary, unconstrained
  individual), infer Γ⊢Δ,∀x.φ.
- **∃-left**: mirrors ∀-right with the same eigenvariable condition, on the left.

A formula is **valid** iff ⊢ φ has a derivation. Worked example — derive ⊢ (p∧q)→p:
```
p, q ⊢ p          (axiom)
──────────────── (→-right)
⊢ (p∧q) → p   (using p∧q,⊢p as Γ after ∧-left-style decomposition folded into the antecedent)
```
(Spelled out fully: from the axiom sequent p ⊢ p, weaken to p,q ⊢ p, then apply ∧-left to combine
p,q into p∧q, giving p∧q ⊢ p, then →-right gives ⊢ (p∧q)→p.)

## 3. First-Order Tableau
Extends Week 4's branching idea to unrestricted FOL. Starting from the negation of the formula to
be proved valid (or directly from a set of formulas to be shown unsatisfiable), apply:
- **∧-rule**: add both conjuncts to the same branch (no branching, as in Week 4's ⊓-rule).
- **∨-rule**: branch into two, one per disjunct (as in Week 4's ⊔-rule).
- **¬¬-rule**: erase a double negation.
- **∀-rule**: from ∀x.φ on a branch, add φ[x/t] for **any** term t already present on the branch
  (may need to be applied more than once with different t's — unlike Week 4's single-pass ∀-rule,
  because FOL terms are not fixed in advance).
- **∃-rule**: from ∃x.φ, add φ[x/c] for a **fresh** constant c not yet used on the branch (applied
  once, exactly as Week 4's ∃-rule created a fresh role-successor).
A branch **closes** on a clash between a literal and its negation (after unifying, if variables
remain free). The formula set is unsatisfiable iff every branch closes.

**Worked example.** Show {∀x.(P(x)→Q(x)), P(a), ¬Q(a)} is unsatisfiable: ∀-rule instantiates
P(a)→Q(a) (t=a); this is ¬P(a)∨Q(a), so ∨-rule branches into ¬P(a) and Q(a). The ¬P(a) branch
clashes with P(a); the Q(a) branch clashes with ¬Q(a). Every branch closes — unsatisfiable, so the
original implication (∀x.(P(x)→Q(x)) ∧ P(a)) → Q(a) is valid.

## 4. Resolution Refinement Strategies
Plain (unrestricted) resolution, as covered in the undergraduate course, allows resolving *any*
two clauses on complementary (unifiable) literals. This is sound and complete but wastes effort
resolving clauses that can never contribute to a refutation. Two standard refinements restrict
*which* resolution steps are allowed while **provably preserving refutation-completeness**:

- **Set-of-support (SOS).** Partition the clause set into S (the *set of support*, typically the
  clauses derived from the negated query/goal) and T = everything else. Require every resolution
  step to use at least one parent that is in S or derived from S. This is complete whenever T
  alone is satisfiable (a property that typically holds when T is the background theory and S is
  the negated goal) — if the whole set is unsatisfiable, a refutation using only SOS-restricted
  steps still exists, because T-only resolvents can never be needed to derive the empty clause.
  It prunes the large, often useless set of resolvents that stay purely within T.
- **Ordering strategies.** Fix a total ordering on literals/terms and only permit resolving on a
  clause's *maximal* literal(s) under that ordering. This eliminates many redundant/symmetric
  derivations of the same resolvent while remaining refutation-complete, because any resolution
  step skipped under the ordering restriction can be shown derivable (or unnecessary) via an
  ordering-respecting alternative path to the empty clause.

```python
def resolve(c1, c2):
    """c1, c2: frozensets of literals, e.g. {('P','x'), ('not','Q','a')}. Returns the set of all
    possible resolvents (propositional case, for clarity; extends to FOL with unification)."""
    resolvents = set()
    for lit in c1:
        neg = ('not',) + lit if lit[0] != 'not' else lit[1:]
        if neg in c2:
            new = (c1 - {lit}) | (c2 - {neg})
            resolvents.add(frozenset(new))
    return resolvents

def sos_resolution(T, S, max_steps=1000):
    """T, S: sets of frozenset clauses. Only resolve pairs with >=1 parent drawn from the
    current support set (S, growing as new resolvents derived from it are added)."""
    support = set(S)
    background = set(T)
    all_clauses = support | background
    steps = 0
    while steps < max_steps:
        steps += 1
        new = set()
        for c1 in support:
            for c2 in all_clauses:
                if c1 == c2:
                    continue
                for r in resolve(c1, c2):
                    if r not in all_clauses:
                        new.add(r)
        if frozenset() in new:
            return True, steps  # empty clause derived -> refutation found
        if not new:
            return False, steps  # saturated, no refutation
        support |= new
        all_clauses |= new
    return None, steps  # inconclusive within max_steps
```

## 5. In-Class/Lab Exercise
Take the clause set {¬P(a)∨Q(a), P(a), ¬Q(a)} (propositional instantiation of §3's example),
designate S = {¬Q(a)} (the negated-goal literal), and trace `sos_resolution` by hand: which
resolvent is derivable first under the SOS restriction, and does it reach the empty clause in
fewer steps than unrestricted resolution would need to consider?

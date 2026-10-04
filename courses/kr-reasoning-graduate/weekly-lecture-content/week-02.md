# Week 2 — Lecture Content: Modal Logic

## 1. Why Modal Logic
Propositional and first-order logic have no way to say "it is *necessary* that p" as distinct
from "p is true," or "agent a *knows* p" as distinct from "p is true." Modal logic adds exactly
this: a **necessity** operator □ ("necessarily," or "agent a knows") and a **possibility**
operator ◇ ("possibly," or "agent a considers possible"), with ◇φ ≡ ¬□¬φ.

## 2. Kripke (Possible-Worlds) Semantics
A **frame** is a pair ⟨W, R⟩: W is a non-empty set of **worlds**, and R ⊆ W×W is the
**accessibility relation** (wRv reads "v is possible/accessible from w"). A **model**
M = ⟨W, R, V⟩ adds a valuation V(p) ⊆ W for every propositional atom p (the worlds where p is
true). The satisfaction relation M,w ⊨ φ is defined by the usual propositional clauses plus:

- M,w ⊨ □φ  iff  for every v ∈ W with wRv, M,v ⊨ φ.
- M,w ⊨ ◇φ  iff  for some v ∈ W with wRv, M,v ⊨ φ.

```python
def satisfies(model, world, formula):
    """model: dict with 'worlds', 'R' (dict world -> set of accessible worlds), 'V' (dict atom -> set of worlds)."""
    op = formula[0]
    if op == 'atom':
        return world in model['V'].get(formula[1], set())
    if op == 'not':
        return not satisfies(model, world, formula[1])
    if op == 'and':
        return satisfies(model, world, formula[1]) and satisfies(model, world, formula[2])
    if op == 'or':
        return satisfies(model, world, formula[1]) or satisfies(model, world, formula[2])
    if op == 'box':
        return all(satisfies(model, v, formula[1]) for v in model['R'].get(world, set()))
    if op == 'diamond':
        return any(satisfies(model, v, formula[1]) for v in model['R'].get(world, set()))
    raise ValueError(f"unknown operator {op}")
```

## 3. Correspondence Theory: Accessibility Properties ↔ Modal Systems
No constraint at all on R gives the minimal normal modal logic **K**, which validates only the
distribution axiom □(φ→ψ)→(□φ→□ψ) and the necessitation rule. Each further property of R
validates one more schema, and the standard named systems stack these properties:

| Property of R | Validated axiom schema | System |
|---|---|---|
| (none) | — | **K** |
| Reflexive (∀w: wRw) | **T**: □φ → φ | **K + T** = "T" |
| Reflexive + transitive | **4**: □φ → □□φ (added to T) | **S4** |
| Reflexive + symmetric + transitive (R an equivalence relation) | **5**: ◇φ → □◇φ (added to S4) | **S5** |

**Why reflexivity validates T.** If wRw, then "φ holds at every v with wRv" (□φ at w) in
particular requires φ to hold at w itself (since w is among the v's) — so □φ → φ.

**Why transitivity validates 4.** Suppose M,w ⊨ □φ and wRu. We must show M,u ⊨ □φ, i.e., for
every v with uRv, φ holds at v. Since wRu and uRv and R is transitive, wRv, so φ holds at v by
the assumption M,w ⊨ □φ. Hence M,w ⊨ □□φ.

**Why an equivalence relation validates 5.** Suppose M,w ⊨ ◇φ, i.e., some u with wRu has
M,u ⊨ φ. Take any v with wRv; by symmetry vRw, and wRu, so by transitivity vRu; since M,u ⊨ φ,
M,v ⊨ ◇φ. As v was arbitrary among R-successors of w, M,w ⊨ □◇φ.

## 4. The Epistemic Reading
Read □ as K_a ("agent a knows that") and ◇ as "agent a considers it possible that." The
accessibility relation wRv now means "world v is indistinguishable from w as far as agent a's
information goes." Indistinguishability is naturally an **equivalence relation** — every world is
indistinguishable from itself (reflexive), if v is indistinguishable from w then w is from v
(symmetric), and indistinguishability chains (transitive) — which is exactly why **S5** is the
standard logic of knowledge: T says you cannot know something false, 4 says you know what you
know, and 5 says you know what you do not know (negative introspection). This is the exact
semantic machinery Week 12 extends to common and distributed knowledge across a group of agents.

## 5. Worked Example
Let W = {w1, w2, w3}, R = {(w1,w2), (w1,w3), (w2,w2), (w3,w3)} (R is not reflexive at w1, so T
need not hold at w1), V(p) = {w2, w3}. Then M,w1 ⊨ □p (both successors w2, w3 satisfy p), but we
cannot conclude M,w1 ⊨ p from this (and indeed p ∉ V's assignment to w1 either way) — illustrating
why T requires reflexivity: without wRw1, □p at w1 says nothing about w1 itself.

## 6. In-Class/Lab Exercise
Using `satisfies` above, build the model from §5 and confirm `satisfies(model, 'w1', ('box', ('atom','p')))` is `True`. Then add the pair `(w1, w1)` to R (making it reflexive at w1) and
re-evaluate `('box', ('atom','p'))` at w1 — it now fails, since p ∉ V(w1), correctly reflecting
that T would be violated if p did not also hold at w1.

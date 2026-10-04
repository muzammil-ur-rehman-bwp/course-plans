# Week 7 — Lecture Content: Belief Revision and Update

## 1. Why Logical Consequence Alone Is Not Enough
Classical logical consequence only ever adds beliefs; it has no account of *giving up* a belief
that turns out to be wrong once new, contradicting information arrives. Belief revision theory
asks: given a current belief set K (closed under logical consequence) and a new sentence φ, what
is the *rational* resulting belief set K∗φ?

## 2. The AGM Postulates
(Alchourrón, Gärdenfors & Makinson, 1985.) K, K∗φ are belief sets (logically closed sets of
sentences); K+φ = Cn(K ∪ {φ}) is plain **expansion** (just adding φ and closing under
consequence, with no attempt to stay consistent).

- **(K∗1)** K∗φ is a belief set.
- **(K∗2)** φ ∈ K∗φ. (**Success** — revision always actually incorporates the new information.)
- **(K∗3)** K∗φ ⊆ K+φ. (Revision adds nothing that plain expansion would not also have added.)
- **(K∗4)** If ¬φ ∉ K, then K+φ ⊆ K∗φ. (When φ does not contradict K, revision coincides exactly
  with expansion — (K∗3)+(K∗4) together give K∗φ = K+φ in the consistent case.)
- **(K∗5)** K∗φ = Cn({⊥}) (the absurd, trivial belief set containing everything) only if ⊨¬φ
  (φ itself is unsatisfiable). (Revision by a *consistent* φ never collapses to triviality.)
- **(K∗6)** If φ ↔ ψ is a logical truth, K∗φ = K∗ψ. (Revision depends only on φ's logical
  content, not its syntactic form.)
- Supplementary: **(K∗7)** K∗(φ∧ψ) ⊆ (K∗φ)+ψ, and **(K∗8)** if ¬ψ ∉ K∗φ then
  (K∗φ)+ψ ⊆ K∗(φ∧ψ) — together, these constrain how revision by a conjunction relates to
  revising by one conjunct and then expanding by the other.

**Why each matters.** (K∗2) rules out an operator that "revises" by simply ignoring φ.
(K∗3)+(K∗4) pin down the *only* interesting case as when φ contradicts K — otherwise revision
must behave exactly like expansion, no more and no less. (K∗5) rules out an operator that always
collapses to absurdity defensively. (K∗6) rules out an operator sensitive to how φ happens to be
written rather than what it means.

## 3. Constructing an AGM-Satisfying Operator: Dalal Revision
Represent a belief set K **semantically** as Mod(K), the set of propositional valuations
(models) satisfying every sentence in K (finite-domain case, as used throughout this course).
**Dalal's revision operator** (Dalal, 1988) defines K∗φ as the belief set whose models are the
models of φ at **minimum Hamming distance** from Mod(K):
```
Mod(K∗φ) = { m ∈ Mod(φ) : HammingDist(m, Mod(K)) is minimal over all m' ∈ Mod(φ) }
```
where HammingDist(m, Mod(K)) = min over n ∈ Mod(K) of the number of propositional atoms on which
m and n disagree.

```python
from itertools import product

def all_valuations(atoms):
    for bits in product([False, True], repeat=len(atoms)):
        yield dict(zip(atoms, bits))

def hamming(m1, m2, atoms):
    return sum(m1[a] != m2[a] for a in atoms)

def models_of(formula_eval, atoms):
    return [m for m in all_valuations(atoms) if formula_eval(m)]

def dalal_revise(K_models, phi_eval, atoms):
    """K_models: list of valuations (dicts) satisfying K. phi_eval: function(valuation)->bool."""
    phi_models = models_of(phi_eval, atoms)
    def dist_to_K(m):
        return min(hamming(m, k, atoms) for k in K_models)
    min_dist = min(dist_to_K(m) for m in phi_models)
    return [m for m in phi_models if dist_to_K(m) == min_dist]
```

## 4. Worked Example
Atoms: {bird, flies}. Suppose K says "Tweety is a bird that flies": Mod(K) = {bird=T, flies=T}
(one model). Now revise by φ = "Tweety is a penguin" encoded as flies=False (penguins do not
fly, kept simple as a single atom flipped) — equivalently φ_eval(m) = not m['flies']. The
φ-models are {bird=T,flies=F} and {bird=F,flies=F}; the first is at Hamming distance 1 from K's
one model, the second at distance 2. Dalal revision picks the minimum-distance model:
K∗φ = {bird=T, flies=False} — Tweety remains a bird but no longer flies, the *minimal* change
consistent with the new information, exactly the intuition AGM revision is meant to formalize.

**Checking (K∗2), (K∗6) on this example** is immediate (flies=False holds in every selected
model; re-phrasing φ equivalently gives the same models). **Checking (K∗4):** since ¬φ (flies=T)
was actually in K here, this example is the genuinely-contradicting case, not the one (K∗4)
constrains — a second example with a non-contradicting φ (e.g., "Tweety has a nest") should be
worked in lab to confirm K∗φ = K+φ in that case.

## 5. Revision vs. Update
**Revision** answers: "What should I now believe about a *static* world, having learned φ?" — it
may force giving up old beliefs that were simply wrong. **Update** (Katsuno & Mendelzon, 1991/92)
answers a different question: "How has the world itself *changed*, given that it now satisfies
φ?" The Katsuno–Mendelzon (KM) update postulates require **minimal change computed separately for
each model of K** (update each possible "way the world could have been" minimally to satisfy φ,
then take the union), not one globally minimal change across all of K as Dalal-style revision
does — because a changing world can legitimately diverge differently from each of K's possible
starting points.

**Worked contrast.** K = {two coins, both showing heads} as two models of a two-coin domain
collapsed to one belief here for brevity: suppose instead K has two models (coin A heads/coin B
heads, and coin A heads/coin B tails) — i.e., we are unsure about coin B. Learn φ = "at least one
coin shows tails." *Revision* reading ("we were wrong about something"): pick whichever of K's
models is closest to a φ-model — likely flipping coin B in the model where it was heads, since
that is a 1-bit change, while the other model needs coin A flipped (also a plausible 1-bit
change) — both are considered under global Dalal revision and the closer one(s) survive.
*Update* reading ("the world changed — someone flipped a coin after we observed it"): apply the
*minimal* change separately to *each* of K's two models to reach a φ-model — from each starting
model, flip whichever coin is needed, and keep *both* resulting updated models, since each
starting world legitimately could have been the one a coin got flipped in. Revision and update
can, and in general do, produce different resulting belief sets from the same K and φ — because
they are correctly modeling different questions.

## 6. In-Class/Lab Exercise
Using `dalal_revise`, reproduce the Tweety example from §4, then construct the two-coin example
from §5 as two separate K_models, revise with Dalal, and discuss (without necessarily coding the
full KM update construction) which of revision's selected models would differ from an update
construction applied per-model.

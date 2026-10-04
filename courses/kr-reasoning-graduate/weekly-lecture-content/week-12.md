# Week 12 — Lecture Content: Multi-Agent Epistemic Reasoning

## 1. From One Agent to a Group
Week 2 gave a single agent's knowledge the Kripke semantics K_aφ (□φ under the epistemic
reading). With a group G of agents, each with their own accessibility relation R_i, three
distinct group-level notions of knowledge arise, each with a different Kripke-model
construction.

## 2. Everyone Knows, Common Knowledge, Distributed Knowledge
- **"Everyone in G knows" (E_Gφ)**: Λ_{i∈G} K_iφ. Modeled by R_E = ⋃_{i∈G} R_i — w,v related
  under R_E iff *some* agent in G cannot distinguish them.
- **Common knowledge (C_Gφ)**: φ holds at every world reachable from w by a *finite chain* of
  individual accessibility steps (any R_i for i ∈ G) — i.e., modeled by the transitive closure of
  R_E. Equivalently, C_Gφ is the infinite conjunction E_Gφ ∧ E_G(E_Gφ) ∧ E_G(E_G(E_Gφ)) ∧ ⋯; it
  is the fixed point satisfying C_Gφ ↔ E_G(φ ∧ C_Gφ). Common knowledge is what a *public*
  announcement, heard by everyone and known by everyone to have been heard by everyone, achieves.
- **Distributed knowledge (D_Gφ)**: modeled by R_D = ⋂_{i∈G} R_i — w,v related under R_D iff
  *every* agent in G fails to distinguish them. Pooling information only ever **shrinks** the set
  of worlds the group cannot jointly distinguish (intersecting constraints is at least as
  restrictive as any one of them), so D_Gφ can hold even though **no individual** agent knows φ —
  it is "what the group would know if members pooled/communicated their individual information,"
  not what anyone currently, individually knows.

**Why these differ.** E_G is the weakest (easiest to achieve: just one agent needing to know
is not even enough — *every* agent must individually know). C_G is strictly stronger than E_G in
general (everyone must know, AND everyone must know that everyone knows, and so on) — this extra
strength is exactly what makes public announcements epistemically powerful even when they convey
no new *factual* information (see §3). D_G is in general incomparable with "any single agent
knows" — it can hold facts no individual knows, by combining each agent's partial information.

## 3. The Muddy Children Puzzle
(Fagin, Halpern, Moses & Vardi, *Reasoning About Knowledge*, Ch. 6; a standard pedagogical
illustration of common knowledge's force.) There are n children; k ≥ 1 of them have mud on their
forehead. Each child sees every *other* child's forehead, but not their own, and does not
initially discuss it. The father truthfully, **publicly**, announces: "at least one of you has a
muddy forehead." He then repeatedly asks, "does any of you know whether you, yourself, are
muddy?" Children answer truthfully and simultaneously.

**Claim (by induction on k):** every child answers "no" for the first k−1 rounds, and on round k,
every muddy child simultaneously answers "yes" (clean children still do not know at round k).

**Base case k=1.** The one muddy child sees everyone else clean; combined with the announcement
("at least one is muddy"), that child immediately deduces they must be the muddy one — answers
"yes" at round 1. Every clean child sees the muddy child and some clean children, which is
consistent with being muddy or clean themselves — they answer "no."

**Inductive step.** Suppose the claim holds for k−1. With k muddy children, each muddy child
sees k−1 other muddy children (and some clean ones). If that muddy child were in fact clean,
there would be exactly k−1 muddy children overall from everyone else's perspective — and by the
inductive hypothesis, those k−1 muddy children would have answered "yes" at round k−1. When they
do *not* (because in the true, k-muddy world they correctly keep answering "no" through round
k−1, since each of them actually sees k−1 *other* muddy children themselves, not k−2), every
muddy child learns at round k that the "k−1 muddy children" hypothesis is false — so they
themselves must be muddy too, and announce "yes" at round k. Clean children, who always see all
k muddy children regardless, gain no equivalent deduction at round k and still answer "no."

**The puzzle's point.** Every child could already *see* who else was muddy — the father's
announcement added no new *factual* information any child didn't already have access to by
looking around. What it added was **common knowledge** that at least one child is muddy — before
the announcement, each child individually knew (by k ≥ 1, if they themselves are clean, k ≥1
others are visibly muddy; if they themselves might be the only muddy one, they could not be sure
anyone else knew it was common knowledge). It is exactly the *common* knowledge (not mere
individual knowledge, nor even E_G at a single level) that powers the induction's "would have
answered yes by round k−1" step — each child's reasoning depends on every other child *also*
reasoning the same way, which is common knowledge's defining extra strength over E_G.

## 4. Simulating the Puzzle via Public-Announcement World Elimination
Model worlds as subsets of {0,...,n−1} (who is muddy). Child i's accessibility relates worlds
that agree on every child except possibly i (child i cannot distinguish "I am muddy" from "I am
clean" holding everything else fixed). The public announcement "at least one is muddy" removes
the empty-set world. Each round's public "no" announcements remove every world at which *some*
child would actually have known their status (since the true rounds' "no" answers rule out any
world where someone would have said "yes" instead).

```python
from itertools import combinations

def all_worlds(n):
    return [frozenset(c) for r in range(n + 1) for c in combinations(range(n), r)]

def indistinguishable(w1, w2, i):
    return (w1 - {i}) == (w2 - {i})

def knows_own_status(world_set, world, i):
    actual = i in world
    for w in world_set:
        if indistinguishable(w, world, i) and (i in w) != actual:
            return False
    return True

def simulate_muddy_children(n, actual_muddy):
    actual = frozenset(actual_muddy)
    W = set(all_worlds(n)) - {frozenset()}       # public announcement: at least one muddy
    t = 0
    while True:
        t += 1
        if all(knows_own_status(W, actual, i) for i in actual):
            return t, W                          # every muddy child now knows, simultaneously
        W = {w for w in W if all(not knows_own_status(W, w, i) for i in range(n))}
```

**Trace for n=3, k=2 (children 0 and 1 muddy, child 2 clean).** Round 1: no child knows (every
muddy or clean child still sees ≥1 other muddy child, consistent with either 1 or 2 total being
muddy) — `simulate_muddy_children` removes the three single-muddy worlds ({0},{1},{2}) at the
round-1 update, since in each of those a child would have known immediately (the base-case
argument of §3). Round 2: within the remaining worlds, each muddy child's only
indistinguishable-and-surviving world is the actual one itself — they now know. The function
returns `t = 2`, matching the hand-derived result.

## 5. Contrast with *Artificial Intelligence* Graduate's Multi-Agent Systems
*Artificial Intelligence*, Graduate studies multi-agent systems through **strategic payoffs** —
normal-form games, Nash equilibrium, mechanism design: what an agent should *do* given its
incentives and others' incentives. This week's lens is entirely about what agents **know or
believe**, and how communication (even communication with no new factual content, as in the
muddy-children announcement) changes it. The two lenses are complementary, not competing — a
fully specified multi-agent system often needs both — but this course owns only the epistemic
side.

## 6. In-Class/Lab Exercise
Run `simulate_muddy_children` for (n=4, k=3) and (n=5, k=1), confirming the returned round
always equals k. Then, for the n=3, k=2 case, additionally check that the clean child (child 2)
does *not* satisfy `knows_own_status` at round 2 within the returned world set, confirming only
the muddy children resolve their uncertainty at round k.

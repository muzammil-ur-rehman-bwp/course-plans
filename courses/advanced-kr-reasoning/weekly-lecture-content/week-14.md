# Week 14 — Lecture Content: Current Open Problems Survey

**This week surveys a fast-moving research area. The three problems below are representative,
not exhaustive or permanent — the instructor refreshes the specific papers cited each offering,
and any of these problems may be substantially advanced, or reframed, between offerings of this
course. Treat this as a snapshot, not a settled conclusion.**

## 1. Scaling Expressive-DL Reasoning and Justification Without Losing Explainability
**The precise obstacle:** SROIQ's worst-case N2ExpTime-completeness (Week 4) is a *worst-case*
bound — real ontologies are usually far better-behaved in practice, which is why production
reasoners (Pellet, HermiT, FaCT++) are usable at all. But as ontologies grow to realistic
industrial scale (tens or hundreds of thousands of axioms), both **reasoning time** and
**justification-finding time** (Week 11's black-box/glass-box methods) degrade in ways that are
not fully characterized by the worst-case complexity class alone — a reasoner can be fast on
*classification* for a large ontology yet become impractically slow on *finding all
justifications* for a single entailment in that same ontology, because justification-finding's
practical cost depends on structural properties (how interconnected the axioms actually are) that
worst-case complexity analysis does not capture. **A named partial approach and its limitation:**
modularity-based justification search (restricting the search to a relevant sub-module, Week 12)
speeds up the common case but is not guaranteed complete — a justification spanning two modules
connected only through a subtle shared term can be missed if the modularization is imperfect, and
detecting that it was missed is itself hard.

## 2. Integrating Probabilistic and Paraconsistent/Many-Valued Semantics
**The precise obstacle:** Week 6's distribution semantics assumes every probabilistic fact is
either present or absent with a well-defined probability — it has no native notion of a fact
being *both* supported and refuted by conflicting independent evidence sources (Week 3's
paraconsistent "B" value). Conversely, Week 3's FDE logic has no native notion of a *graded*
degree of confidence — only the four discrete evidence-combinations. A formalism that is
simultaneously probabilistic *and* genuinely paraconsistent (tolerating contradictory evidence
about an uncertain fact without either exploding or discarding the uncertainty) requires deciding
what a "probability of B" even formally means — and the few proposals in this direction disagree
on the right semantics, with no consensus answer. **A named partial approach and its limitation:**
some proposals assign *separate* probability distributions to "support for true" and "support for
false" evidence (a direct probabilistic lift of FDE's two-coordinate representation, Week 3 §4) —
this is formally coherent, but it is not yet clear how to ground such a joint distribution in
real, elicitable data (where would two independent probability estimates per fact actually come
from in a real knowledge-engineering pipeline?).

## 3. Guaranteeing Neuro-Symbolic Systems Respect Symbolic Constraints
**The precise obstacle:** Week 7's differentiable-logic losses and Week 8's embedding-plus-
constraint hybrids make a trained model *tend toward* satisfying symbolic constraints on the
training distribution, but gradient descent minimizing a soft-logic loss provides **no formal
guarantee** that the constraint holds exactly, nor that it generalizes to inputs outside the
training distribution — a model can achieve near-zero soft-logic loss on its training data while
still violating the hard, exact version of the constraint on novel inputs. Closing this gap would
mean either a training procedure with a *provable* exact-satisfaction guarantee (not merely a
low expected soft-loss), or a principled characterization of exactly how much soft-loss
corresponds to how much exact-constraint violation risk — neither currently exists in general.
**A named partial approach and its limitation:** constrained-output projection (forcibly snapping
a trained model's output onto the nearest point satisfying the hard constraint, as a post-
processing step) guarantees exact satisfaction by construction, but can degrade the model's
learned accuracy arbitrarily if the projection is far from what the model actually wanted to
output, and provides no insight into *why* the unconstrained model disagreed with the constraint
in the first place.

## 4. Connecting Open Problems to Capstone Scoping
A capstone problem statement that is actually a restatement of one of these three problems in
full generality is almost certainly **too large** for a one-semester proposal (these are exactly
the problems the field as a whole has not solved); a problem statement that could be fully
resolved by material already covered in this course is almost certainly **too small**. The right
scope sits between: a specific, falsifiable sub-question connected to one of these threads (e.g.,
not "solve neuro-symbolic guarantee generally" but "does constrained-output projection's accuracy
degradation, for this specific family of constraints, correlate predictably with a measurable
property of the unconstrained model's output distribution?").

## 5. In-Class/Lab Exercise
For one of the three open problems above, state its precise technical obstacle in your own words
(not a copy of the paragraph here) and name one additional partial approach not listed above
(drawn from the instructor-provided current reading), with its limitation. Then write 3–5
sentences connecting this open problem to your own capstone direction: does your problem statement
sit at an appropriately specific scope relative to this broader, unsolved problem?

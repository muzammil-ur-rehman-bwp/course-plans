# Lab Notes 2 — Kripke Model Satisfaction Checker

**Concept recap:** M,w ⊨ □φ iff φ holds at every R-successor of w; M,w ⊨ ◇φ iff φ holds at some
R-successor; reflexivity/transitivity/symmetry of R correspond to axioms T/4/5 respectively.

**Common pitfalls:**
- **Confusing accessibility direction**: `R[w]` should be the set of worlds *accessible from* w
  (i.e., v such that wRv), not the set of worlds that can reach w — getting this backward
  silently inverts every □/◇ evaluation.
- **Vacuous truth of □ at a world with no successors**: if `R.get(world, set())` is empty, `all(...)`
  over an empty set is `True` — so □φ is trivially true at a dead-end world for *any* φ, including
  φ = False. This is mathematically correct but surprising; confirm you understand why before
  treating it as a bug.
- Forgetting that axiom 5 (◇φ→□◇φ) requires the FULL equivalence-relation property (reflexive +
  symmetric + transitive together), not symmetry alone — Task C is designed to catch a student
  who adds symmetry without transitivity and is surprised axiom 5 still fails on some models.
- In Task D, mixing up which accessibility relation (agent a's or agent b's) governs each nested
  K operator — K_a(K_b(p)) at w requires evaluating K_b(p) using agent b's relation at every
  world a cannot distinguish from w, not agent a's relation throughout.

**Debugging tip:** always re-run the exact worked example from the Week 2 lecture content first
— if your `satisfies` function does not reproduce that known-correct result, nothing built on top
of it (Tasks B–D) can be trusted yet.

**Instructor tip:** have students explicitly write out, in English, what each accessibility
relation property *means* epistemically (reflexive = "I cannot have ruled out the actual world";
symmetric = "if v is possible from w, w is possible from v"; transitive = "possibility chains
don't add new possibilities") before coding Task C — the axioms stop feeling arbitrary once
students see guess the epistemic reading first.

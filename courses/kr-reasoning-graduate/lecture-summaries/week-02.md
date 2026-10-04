# Week 2 Summary — Modal Logic

**Key takeaways:**
- A Kripke model ⟨W, R, V⟩ gives □φ ("necessarily φ") the semantics "φ holds at every
  R-accessible world," and ◇φ ("possibly φ") "φ holds at some R-accessible world."
- Properties of R correspond to modal axioms: reflexivity → T (□φ→φ), + transitivity → S4
  (adds □φ→□□φ), + symmetry (R an equivalence relation) → S5 (adds ◇φ→□◇φ).
- Reading □ as "agent a knows" makes S5 the standard logic of knowledge, because
  indistinguishability of worlds is naturally an equivalence relation.

**You should now be able to:** evaluate a modal formula against a finite Kripke model by hand and
in code; determine, from an accessibility relation's properties, which modal system (K, T, S4, S5)
it validates.

**Next week:** Temporal logic — LTL's next/always/eventually/until operators over an execution
trace, a conceptual look at CTL's branching-time operators, and applications to planning and
verification.

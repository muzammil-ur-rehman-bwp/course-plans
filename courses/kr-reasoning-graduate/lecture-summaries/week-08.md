# Week 8 Summary — Argumentation Frameworks; Midterm Review

**Key takeaways:**
- A Dung AF ⟨A, →⟩ abstracts arguments down to an attack relation; S is admissible iff
  conflict-free and defends every one of its members.
- The grounded extension is the unique least fixed point of the characteristic function
  F(S) = {a : S defends a}, computed by iterating F from ∅; it is always a subset of every
  preferred (maximal admissible) extension.
- On a simple mutual-attack pair, the grounded extension is empty while two distinct non-empty
  preferred extensions exist — the two semantics genuinely diverge.

**You should now be able to:** compute the grounded extension of a small AF by hand and in code;
identify preferred extensions; explain why grounded and preferred extensions can diverge on a
mutual-attack example.

**Next week:** Midterm Exam (Weeks 1–8), followed by Markov Logic Networks in depth — the
log-linear model over possible worlds, grounding, and conceptual inference.

# Week 14 Summary — Explainability and Reasoning

**Key takeaways:**
- A proof tree records the full recursive justification for a derived fact — every internal
  node a rule application, its children the facts that satisfied that rule's premises, down to
  leaf facts stated outright.
- A "why not" explanation recurses through a failing query's rule chain to report the specific
  missing leaf fact(s) that blocked derivation.
- Symbolic explanation is exact and faithful by construction (the proof tree *is* the
  computation); post-hoc ML explanation methods only approximate a black-box model's local
  behavior, with no faithfulness guarantee.

**You should now be able to:** generate a full proof tree and a recursive "why not" explanation
over a small rule base; state precisely what guarantee a proof tree provides that a post-hoc ML
explanation does not.

**Next week:** Research methods and project work session — how to read and critique a KR paper,
and structured capstone work time; Paper Critique due.

# Week 9 Summary — Midterm Exam; Non-Monotonic Reasoning

**Key takeaways:**
- Classical logic is monotonic — once `KB ⊨ α`, no additional information can ever retract `α`
  — which is the wrong model for everyday common-sense reasoning with exceptions.
- The closed-world assumption treats any non-derivable atomic fact as false, not unknown; default
  logic formalizes defeasible rules whose conclusions apply only while their justification is not
  contradicted by what is currently known.
- The same default rule set can produce fewer conclusions once new, conflicting facts are added —
  genuine retraction, impossible under strict rules or resolution.

**You should now be able to:** state what monotonicity means and why it fails for common-sense
reasoning; implement a CWA query engine and a default-logic extension builder; construct a case
where adding a fact retracts a previously derived conclusion.

**Next week:** planning in depth — STRIPS revisited, partial-order planning, and planning graphs.

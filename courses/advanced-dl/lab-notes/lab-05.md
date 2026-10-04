# Lab Notes 5 — Induction Heads and the Implicit-Gradient-Descent Analogy

**Concept recap:** induction heads are a well-supported mechanistic finding (structural weight
analysis plus ablation evidence); the implicit-gradient-descent analogy is rigorously shown only
for restricted linear-attention/linear-regression toy settings — the central skill this week is
evidence-grading, not memorizing either result.

**Common pitfalls:**
- In Task A, off-by-one errors in `toy_induction_trace` — the induction target is the position
  *right after* the most recent prior occurrence, not the occurrence itself; a common bug points
  one position too early.
- In Task D, writing a critique that simply restates the implicit-gradient-descent claim more
  confidently rather than applying the evidence-grading checklist — the graded skill is precisely
  separating what is proven from what is suggested, not summarizing the topic.
- Treating "this hasn't been proven for real Transformers" as equivalent to "this is false" — the
  correct, precise claim is that the general version remains an open question (reused explicitly
  in Week 14), not that the toy equivalence is wrong.
- In Task B, mistaking any attention weight concentration for "an induction head" — confirm the
  specific previous-token-head-then-induction-head two-step structure, not just that *some* head
  attends somewhere non-trivial.

**Debugging tip:** validate `toy_induction_trace` on a hand-constructed 4-token sequence you can
verify by eye before trusting it on longer, randomly generated sequences.

**Instructor tip:** in grading Task D, explicitly reward critiques that correctly state what
*would* count as evidence for the general claim, even if (correctly) concluding the evidence does
not yet exist — this is the harder and more valuable half of the skill, and distinguishes genuine
evidence-grading from simple skepticism.

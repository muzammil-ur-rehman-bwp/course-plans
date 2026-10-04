# Lab Notes 1 — Environment Setup & Foundations Diagnostic

**Concept recap:** this course assumes the graduate KR&R course's ten core results in full; the
diagnostic is a self-check, not new instruction — any gap found here should be independently
closed this week, since Week 2 onward builds directly on this foundation without re-teaching it.

**Common pitfalls:**
- Treating the diagnostic as a quiz to pass rather than a genuine self-check — students who write
  a vague, hedged sentence for a topic they do not actually remember well should flag it honestly,
  since the gap will resurface (e.g., in Week 4's SROIQ, which assumes the ALC tableau fluently)
  rather than disappearing.
- Installing PyTorch for the first time in Week 7 instead of now — PyTorch installation issues
  (CUDA/CPU build mismatches, version conflicts with NumPy) are far easier to resolve in Week 1's
  low-stakes setup lab than during Week 7's graded lab.
- In Task C, naming the *topic* area of a sibling course instead of the specific course title —
  "this isn't about machine learning" is too vague; name the specific sibling course (e.g.,
  "Advanced Artificial Intelligence" for a regret-theory question).

**Debugging tip:** if PyTorch import fails, verify the installed build matches your Python
version and platform (CPU-only builds are suffficient for this course; no GPU is required for any
graded component).

**Instructor tip:** collect (anonymized) which of the 10 diagnostic topics students flagged most
often, and spend 10–15 minutes of the Week 2 session on the most commonly flagged gap before
moving to new material — a quick, cheap way to catch foundation gaps before they compound.

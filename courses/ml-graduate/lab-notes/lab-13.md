# Lab Notes 13 — Simpson's Paradox and Confounding

**Concept recap:** Simpson's paradox arises when an unequally-distributed confounder (here,
severity) causes an aggregate trend to reverse every one of its own within-stratum trends; the
stratified comparison, not the aggregate one, isolates the treatment's actual effect — unless the
stratifying variable is itself a mediator, in which case stratifying would be the wrong move.

**Common pitfalls:**
- **Confusing correlation with causation** when interpreting the aggregate numbers alone — the
  raw, unstratified recovery-rate comparison in Section 3 of the lecture content *looks like* a
  complete, valid comparison of the two treatments; it is not, and this lab exists specifically
  to make that gap vivid with real numbers.
- Assuming stratifying by *any* available variable is always the safe, conservative choice — if
  the stratifying variable is a **collider** or a **mediator** rather than a confounder,
  stratifying can introduce bias rather than remove it (Week 13, Section 4) — always classify the
  variable's causal role first, as Task D requires.
- Constructing a Task C "original example" so similar to the lecture's that it does not actually
  test understanding of *why* the reversal happens (e.g., just relabeling "Treatment A/B" and
  "mild/severe") — the instructor will check for a genuinely different confounding context.

**Debugging tip:** if your Task C reversal doesn't appear, check that the confounder is
**correlated with treatment assignment** (one treatment group must be weighted more heavily toward
one stratum) — Simpson's paradox requires this unequal mix; a confounder that happens to be evenly
split across both treatment groups will not produce a reversal.

**Instructor tip:** have students compute the *overall* sample sizes per treatment stratum
explicitly (as in the lecture's table) before looking at recovery rates — seeing the 263-vs-80
severe-case imbalance up front makes the mechanism, not just the paradoxical conclusion, visible.

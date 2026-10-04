# Lab Notes 10 — Evolutionary and Differentiable Neural Architecture Search

**Concept recap:** evolutionary search uses mutation and selection with no gradients;
differentiable NAS relaxes a discrete operation choice into a learnable softmax mixture, trained
jointly with ordinary gradient descent and discretized via arg-max at the end.

**Common pitfalls:**
- Using a fitness function that is too expensive (too many training steps per candidate) in
  Task B — the whole point of a cheap performance-estimation strategy is speed; if a single
  generation takes minutes rather than seconds, the search-cost comparison in Task D loses its
  force.
- In Task C, initializing `alpha` to strongly favor one operation from the start, which can make
  it hard to tell whether the relaxation is "learning" a preference or simply confirming an
  initialization bias — initialize at or near zero (uniform mixture) as in the lecture code.
- Confusing differentiable NAS's continuous training phase with its final discrete architecture —
  the trained `MixedOp`'s *mixture* is not the final model; only after `discretize()` is called
  (keeping the arg-max operation) is the actual searched architecture obtained.
- In Task D, comparing search costs using wall-clock time on different machines/sessions rather
  than a consistent unit (e.g., total forward/backward passes), making the comparison unreliable.

**Debugging tip:** before running the full evolutionary search, verify `mutate` on a single
architecture encoding actually changes at least one position at the chosen mutation rate, and
leaves others at their original value most of the time.

**Instructor tip:** have students predict, before running Task C, which of the three `MixedOp`
candidate operations will "win" (have the largest final softmax weight) on their specific toy
task, and discuss afterward whether the result matches a reasonable prior guess about which
operation should suit the task — this connects the abstract relaxation mechanism to an
interpretable, checkable outcome.

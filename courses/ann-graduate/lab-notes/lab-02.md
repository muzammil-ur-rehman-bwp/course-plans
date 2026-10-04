# Lab Notes 2 — Universal Approximation

**Concept recap:** the Universal Approximation Theorem guarantees a shallow network *can*
represent any continuous target to arbitrary precision given enough units; it says nothing about
how many units are needed for a specific target, or whether depth can represent the same target
with fewer total units.

**Common pitfalls:**
- Expecting MSE to decrease *smoothly* and *monotonically* with width — with a fixed, finite
  number of gradient-descent training steps, a wider network needs more steps (or a tuned
  learning rate) to actually converge, so an apparent non-monotonic width-vs-error curve is often
  an optimization artifact, not evidence against the theorem.
- Choosing a target function too simple (e.g., a single sine wave) for Task C — depth's advantage
  over width is only visible on targets with genuinely multiple length scales or repeated
  piecewise structure (hence the square-wave suggestion).
- Conflating "the narrow-deep network trained worse in my 500-epoch budget" with "narrow-deep
  networks are less expressive" — expressivity (what a class *can* represent) and trainability
  (what gradient descent *actually finds* in finite time) are different questions; this
  distinction recurs all semester (Weeks 9–12 especially).

**Debugging tip:** if the wide-shallow network's MSE does not improve past a plateau, check the
learning rate and epoch count before concluding anything about expressivity.

**Instructor tip:** use this lab to plant, early, the "representable vs. trainable vs.
generalizable" three-way distinction that Weeks 9–12 will repeatedly return to.

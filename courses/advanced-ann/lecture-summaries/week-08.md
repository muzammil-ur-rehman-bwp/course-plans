# Week 8 Summary — Scaling Laws; Midterm Review

**Key takeaways:**
- Test loss follows an approximate power law in model size, data size, and compute, remarkably
  stable across orders of magnitude (Kaplan et al.); Hoffmann et al.'s compute-optimal refinement
  shows model and data size should be scaled together, not model size alone, under a fixed budget.
- Theoretical attempts to explain the power-law form (data-manifold/intrinsic-dimension arguments;
  random-feature/kernel-theoretic connections to this course's own NTK material) are partial, not
  complete, first-principles explanations of the specific observed exponents.
- Weeks 1–8 reviewed for the midterm: NTK revisited, mean-field theory, implicit bias, feature
  learning, sharpness/SAM, grokking, and scaling laws.

**You should now be able to:** fit a power-law exponent to loss-vs-size data; state what the
compute-optimal refinement corrects; explain which parts of the scaling-law phenomenon are
theoretically understood versus still open; synthesize Weeks 1–8 at a qualifying-exam standard.

**Next week:** Midterm Exam, followed by statistical-physics approaches to neural network theory —
the replica-method idea and spin-glass analogies for loss-landscape structure.

# Week 10 Summary — Double Descent Revisited Rigorously

**Key takeaways:**
- The interpolation threshold (effective capacity just enough to fit the training set exactly)
  organizes double descent across three axes: model size, sample size, and training time/epochs
  (Nakkiran et al.'s broader, rigorous characterization).
- Random-matrix-theoretic analysis of tractable linear/random-feature models gives a fully rigorous
  account of double descent's existence and location in those cases, connecting back to this
  course's own NTK/kernel material.
- Current theory explains the peak's *location* well but is considerably less complete about its
  exact *magnitude* for realistic, finite-width, feature-learning deep networks.

**You should now be able to:** state double descent across all three axes using one organizing
concept; reproduce a model-size double-descent curve; state precisely what current theory does and
does not explain about the phenomenon's magnitude.

**Next week:** Modern generalization bounds — why classical VC/Rademacher-style bounds are often
vacuous for deep networks, and PAC-Bayes bounds as a tighter alternative framework.

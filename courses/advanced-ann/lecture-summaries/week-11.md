# Week 11 Summary — Modern Generalization Bounds — PAC-Bayes

**Key takeaways:**
- Classical VC/Rademacher bounds are worst-case, class-uniform, and routinely vacuous (exceed 1)
  for deep networks at realistic scale.
- PAC-Bayes bounds instead compare a learned posterior $Q$ to a data-independent prior $P$; the
  bound's tightness depends on $\mathrm{KL}(Q\|P)$, not raw parameter count.
- A Gaussian-perturbation posterior centered at the trained weights connects PAC-Bayes directly to
  Week 6's sharpness material: flat minima tolerate larger posterior variance, tightening the
  bound; sharp minima force smaller variance, loosening it.

**You should now be able to:** explain why classical bounds go vacuous for deep networks; state
the PAC-Bayes bound's structure; compute a simple PAC-Bayes bound under a Gaussian posterior and
explain its connection to sharpness.

**Next week:** Loss-landscape geometry — mode connectivity, and the Lottery Ticket Hypothesis
revisited with current refinements and critiques.

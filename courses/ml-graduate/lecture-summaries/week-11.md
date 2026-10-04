# Week 11 Summary — Structured Prediction: Conditional Random Fields

**Key takeaways:**
- Structured prediction models a whole labeled sequence jointly, capturing dependencies between
  adjacent labels that independent per-token prediction would ignore.
- A linear-chain CRF models $p(y\mid x)$ directly (discriminative), using arbitrary, overlapping
  feature functions of $x$; an HMM models the joint $p(x,y)$ (generative) via transition and
  emission probabilities.
- This discriminative/generative distinction is the key practical contrast between CRFs and HMMs
  — it is not a treatment of general PGM inference, which belongs to *Artificial Intelligence*
  (Graduate).
- CRF training maximizes a concave conditional log-likelihood, solvable by the convex-optimization
  machinery of Week 6.

**You should now be able to:** state the linear-chain CRF model, contrast it with an HMM, and fit
a CRF to a sequence-labeling task.

**Next week:** dimensionality reduction theory — PCA re-derived rigorously, and kernel PCA.

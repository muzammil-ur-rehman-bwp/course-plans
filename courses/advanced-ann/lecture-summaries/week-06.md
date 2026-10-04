# Week 6 Summary — Sharpness and Generalization

**Key takeaways:**
- Sharpness (Hessian top-eigenvalue or a perturbation-based proxy) is measured as a candidate
  explanation for why some minima generalize better than others.
- The reparameterization critique — ReLU's positive homogeneity lets function-preserving rescalings
  change measured sharpness — is a serious, unresolved objection to a purely causal reading of the
  sharpness-generalization correlation.
- SAM derives a practical two-step update from the min-max worst-case-loss objective via a
  first-order approximation, and its empirical success is real but does not, by itself, settle the
  causal debate.

**You should now be able to:** compute a Hessian top-eigenvalue estimate via power iteration;
derive SAM's update from its min-max objective; state precisely what SAM's results do and do not
establish about the flat-minima hypothesis.

**Next week:** The grokking phenomenon — delayed generalization long after training accuracy
saturates, and the current competing hypotheses for why it occurs.

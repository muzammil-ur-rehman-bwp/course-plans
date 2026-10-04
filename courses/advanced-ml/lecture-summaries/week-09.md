# Week 9 Summary — Causal Inference in Depth II: Instrumental Variables and Do-Calculus

**Key takeaways:**
- Instrumental variables identify a causal effect despite unobserved confounding, given
  relevance, the exclusion restriction, and independence from the confounder; the linear case
  gives $\beta = \mathrm{Cov}(Z,Y)/\mathrm{Cov}(Z,T)$, estimated via two-stage least squares.
- The do-calculus's three rules (observation insertion/deletion, action/observation exchange,
  action insertion/deletion) are each a $d$-separation statement in a surgically modified graph,
  and are sound and complete for identifying $P(y\mid\mathrm{do}(x))$ when identification is
  possible at all.
- The back-door adjustment formula is derivable from the do-calculus rules rather than asserted;
  some graphs (unobserved confounder, no valid adjustment set or instrument) are provably
  non-identifiable.

**You should now be able to:** derive the linear-IV identification formula; state and apply the
three do-calculus rules to determine identifiability; implement a from-scratch 2SLS IV estimator.

**Next week:** Distribution shift and domain adaptation — covariate shift formalized, importance
weighting, and generalization guarantees under shift.

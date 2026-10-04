# Week 2 Summary — PAC Learning in Depth

**Key takeaways:**
- PAC learnability formalizes "learnable" as: polynomially many samples suffice to guarantee
  $L_D(\hat h)\leq\epsilon$ with probability $\geq1-\delta$.
- The finite-class bound $m\geq\frac{1}{2\epsilon^2}\ln\frac{|H|}{\delta}$ follows from bounding
  one bad hypothesis via Hoeffding's inequality, then a union bound over all of $H$.
- Sample complexity depends only *logarithmically* on $|H|$ and $1/\delta$ — this is the precise
  sense in which a finite hypothesis class is "simple."
- The agnostic, two-sided version of this bound is a uniform-convergence statement — the general
  template every later bound (VC, Rademacher) specializes.

**You should now be able to:** derive the finite-class sample-complexity bound step by step and
compute it for given $|H|,\epsilon,\delta$.

**Next week:** VC dimension — extending this theory to infinite hypothesis classes via shattering.

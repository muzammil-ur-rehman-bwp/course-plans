# Week 3 Summary — VC Dimension in Depth

**Key takeaways:**
- VC dimension is the largest $m$ for which *some* $m$-point set is shattered — proving it exactly
  requires both a shattering construction (lower bound) and an impossibility argument for one
  larger set size (upper bound).
- Worked proofs: $\mathrm{VCdim}(\text{intervals on }\mathbb{R})=2$; $\mathrm{VCdim}(\text{halfspaces
  in }\mathbb{R}^d)=d+1$, the latter via Radon's theorem.
- The VC generalization bound replaces $\log|H|$ with (a log factor times) $\mathrm{VCdim}(H)$,
  extending Week 2's theory to infinite, continuously-parameterized classes.
- The Sauer–Shelah lemma — polynomial growth-function collapse past $m=\mathrm{VCdim}(H)$ — is the
  mechanism that makes this extension possible.

**You should now be able to:** prove the VC dimension of a hypothesis class by exhibiting a
shattered set and a complete impossibility argument for the next size up.

**Next week:** Rademacher complexity — a data-dependent capacity measure that can tighten, and
provably generalizes, the VC-dimension-based bound.

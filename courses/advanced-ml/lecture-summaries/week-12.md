# Week 12 Summary — Robust Statistics

**Key takeaways:**
- The sample mean has breakdown point $1/n\to0$ and only Chebyshev-type ($1/\delta$) concentration
  under heavy tails; the trimmed mean has breakdown point $\epsilon$.
- Median-of-means, via per-group Chebyshev concentration plus a Chernoff-boosted majority-vote
  argument across $k=O(\log(1/\delta))$ groups, achieves sub-Gaussian-type
  $O(\sigma\sqrt{\log(1/\delta)/n})$ error under only a finite-variance assumption.
- Both estimators degrade gracefully, with bounded error, under $\epsilon$-fraction adversarial
  contamination, unlike the sample mean's unbounded error — a structural analogy to adversarial
  robustness in modern ML.

**You should now be able to:** derive the median-of-means error bound; implement trimmed-mean and
median-of-means estimators; empirically confirm their bounded error under heavy tails and
adversarial contamination where the sample mean fails.

**Next week:** Research methods for statistical learning theory at the postgraduate level —
reading cutting-edge theory papers, identifying open problems, and structured capstone work time.

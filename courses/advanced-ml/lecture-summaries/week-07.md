# Week 7 Summary — Nonparametric Bayesian Methods II: The Chinese Restaurant Process

**Key takeaways:**
- The CRP seats customer $n+1$ at existing table $k$ with probability $n_k/(n+\alpha)$ or a new
  table with probability $\alpha/(n+\alpha)$ — the combinatorial twin of the Dirichlet process.
- The CRP's induced partition is exchangeable: its probability depends only on block sizes, not
  arrival order, licensing its use as a model for unordered data.
- The expected number of occupied tables grows as $O(\alpha\log n)$, formalizing how a DP
  mixture's effective cluster count grows slowly with more data.

**You should now be able to:** state the CRP seating rule and derive the $O(\alpha\log n)$
growth bound; explain exchangeability and why it matters for valid inference; implement a CRP
sampler and a CRP-based infinite Gaussian mixture generator.

**Next week:** Causal inference in depth I — the potential-outcomes framework, formalizing
confounding, and propensity-score methods; midterm review.

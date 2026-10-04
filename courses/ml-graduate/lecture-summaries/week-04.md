# Week 4 Summary — Rademacher Complexity

**Key takeaways:**
- Empirical Rademacher complexity $\widehat{\mathcal{R}}_S(H)$ measures how well $H$ can correlate
  with pure random noise on the *actual* sample — a data-dependent, potentially much tighter
  alternative to the worst-case VC bound.
- Massart's lemma, combined with the Sauer–Shelah growth-function bound, shows the VC bound is
  recoverable as a special case of the (tighter) Rademacher bound.
- For a norm-bounded linear class, Rademacher complexity has a clean closed form that is
  *dimension-free* — a key advantage over VC dimension for high-dimensional linear classes.

**You should now be able to:** state the Rademacher generalization bound, compute Rademacher
complexity for a norm-bounded linear class, and explain its relationship to VC-based bounds.

**Next week:** concentration inequalities — deriving the Hoeffding and McDiarmid tools that
Weeks 2–4 used as black boxes.

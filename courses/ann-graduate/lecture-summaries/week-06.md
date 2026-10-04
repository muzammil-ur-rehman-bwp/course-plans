# Week 6 Summary — Optimization Landscape Theory I

**Key takeaways:**
- A critical point's Hessian eigenvalue signs classify it; in high dimensions, having all
  eigenvalues the same sign (a true local min/max) becomes exponentially unlikely, so saddle
  points — not bad local minima — dominate the high-dimensional loss landscape.
- Plain Newton's method can be actively attracted to saddle points, since it rescales every
  eigendirection by magnitude alone, ignoring sign.
- Newton's method and Gauss-Newton converge fast locally but require $O(p^2)$ storage and
  $O(p^3)$ inversion cost, ruling them out exactly at neural-network scale.

**You should now be able to:** classify a critical point from its Hessian eigenvalues and explain
why second-order methods are theoretically appealing yet practically infeasible for large networks.

**Next week:** optimization-landscape theory II — Adam's convergence subtleties, natural gradient
descent, and learning-rate warmup theory.

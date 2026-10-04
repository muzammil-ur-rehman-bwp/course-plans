# Week 11 Summary — Temporal and Spatial Reasoning

**Key takeaways:**
- Allen's interval algebra defines thirteen mutually exclusive, jointly exhaustive relations
  between any two time intervals, computed directly from their start/end points.
- Path consistency propagates known relations through triples of intervals using a composition
  table, exactly analogous to arc consistency for CSPs; an empty resulting relation set signals
  an inconsistent temporal network.
- Basic topological spatial relations (disjoint, touches, overlaps, contains), formalized by
  calculi such as RCC-8, are the spatial analogue of Allen's algebra.

**You should now be able to:** compute the Allen relation between two intervals; implement path
consistency over a small temporal constraint network; describe basic spatial relations and their
relationship to Allen's algebra.

**Next week:** probabilistic reasoning and Bayesian networks in depth — exact inference by
variable elimination. Assignment 3 assigned; capstone proposal due.

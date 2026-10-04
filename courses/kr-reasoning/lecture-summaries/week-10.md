# Week 10 Summary — Planning in Depth

**Key takeaways:**
- STRIPS action schemas with variables are grounded into concrete actions over a domain's
  objects, generating a reusable action set rather than one hand-written per problem.
- Partial-order planning adds steps only to resolve open preconditions, tracks causal links, and
  detects/repairs threats, avoiding premature commitment to a total action order.
- A planning graph alternates proposition and action levels and tracks mutex relations; GraphPlan
  extracts a plan by searching backward through it once the goals appear non-mutex at some level.

**You should now be able to:** ground a variabilized STRIPS schema over a set of objects;
implement a small partial-order planner that resolves open preconditions and detects threats;
build the first levels of a planning graph by hand and identify mutex pairs.

**Next week:** temporal and spatial reasoning — Allen's interval algebra and basic spatial
relations. Capstone proposal due.

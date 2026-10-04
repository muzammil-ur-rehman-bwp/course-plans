# Lab Notes 10 — Distance-Based Belief-Merging Evaluator

**Concept recap:** the merged result's models minimize an aggregate (sum or max) of each
valuation's distance to every agent's closest model, restricted to integrity-constraint-
satisfying valuations — a direct, symmetric generalization of Dalal revision to a profile of
peer belief sets.

**Common pitfalls:**
- Forgetting to restrict the search to `Mod(IC)` **before** minimizing distance — computing the
  global minimum-distance valuation and only *afterward* checking whether it satisfies IC can
  silently return an invalid result if the unconstrained minimum does not happen to satisfy IC.
- Confusing "distance to the profile" with "distance to the union of all agents' models treated
  as one set" — `distance_to_set` must be called **per agent** (`d(v, Ki)` for each i separately)
  and then aggregated; collapsing all agents' models into one set before computing distance loses
  exactly the per-agent structure the merging operator is supposed to respect.
- In Task C, mis-identifying which valuation differs between sum and max aggregation — print
  every IC-satisfying valuation's full per-agent distance list (not just the aggregate score) to
  see precisely which agent's distance is driving each aggregation's choice.

**Debugging tip:** for a toy 2-variable, 2-agent case small enough to enumerate by hand (4
valuations), compute every valuation's distance to each agent's models manually before trusting
`merge`'s output on the full Task B/C example.

**Instructor tip:** ask students to construct, as an extension, a profile where sum and max
aggregation give *disjoint* result sets (not just a differing subset) — a sharper illustration of
how much the aggregation choice itself encodes a fairness judgment.

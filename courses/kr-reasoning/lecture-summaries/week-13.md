# Week 13 Summary — Reasoning with Uncertainty Beyond Bayes

**Key takeaways:**
- Markov logic networks attach weights to first-order formulas, making them soft constraints: a
  world satisfying more (weighted) formula groundings is more probable, but none is strictly
  forbidden unless its weight is infinite.
- Fuzzy logic generalizes set membership to a degree in `[0, 1]`; fuzzy AND/OR/NOT generalize
  Boolean operations via `min`/`max`/`1 - x`.
- A fuzzy rule's firing strength combines its antecedents' membership degrees (typically via
  fuzzy AND), which then scales its contribution to a combined, later-defuzzified output.

**You should now be able to:** explain MLNs conceptually and work a small weighted-formula
comparison by hand; implement triangular membership functions and fuzzy set operations; evaluate
a small fuzzy rule's firing strength on a concrete input.

**Next week:** knowledge-based agents in practice — an integrated forward/backward-chaining
reasoner over a toy knowledge base. Assignment 4 assigned.

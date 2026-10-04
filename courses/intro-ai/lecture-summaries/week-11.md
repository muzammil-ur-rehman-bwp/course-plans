# Week 11 Summary — Bayesian Networks

**Key takeaways:**
- A Bayesian network represents a joint distribution compactly as a directed acyclic graph of
  conditional probability tables, exploiting conditional independence encoded by graph structure.
- Inference by enumeration computes `P(query | evidence)` by summing the joint probability
  (expressed as a product of CPT entries) over hidden variables, then normalizing.
- The graph structure directly tells you what is and is not conditionally independent — a
  separate skill from computing the actual probabilities.

**You should now be able to:** read a Bayesian network's CPTs; compute a query probability by
enumeration on a small network; identify conditional independencies from graph structure.

**Next week:** introduction to machine learning (survey) — supervised vs. unsupervised learning,
and a decision-tree worked example.

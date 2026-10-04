# Week 14 Summary — Recommender Systems

**Key takeaways:**
- Content-based filtering recommends items similar to ones a user liked, using item feature
  vectors and cosine similarity; it has no item cold-start problem but is limited by the chosen
  features.
- Collaborative filtering uses the user-item ratings matrix; user-based filtering finds similar
  users, item-based filtering finds similar items — item-based similarities tend to be more
  stable in practice.
- Cosine similarity, computed with `sklearn.metrics.pairwise.cosine_similarity` or
  `NearestNeighbors(metric="cosine")`, underlies both content-based and collaborative approaches
  in this course's treatment.
- Real systems often blend content-based and collaborative signals (hybrid recommenders).

**You should now be able to:** compute cosine similarity between feature/rating vectors; build a
simple content-based and item-based collaborative recommender.

**Reminder:** Assignment 4 (unsupervised learning, feature engineering & recommenders) was
assigned this week.
**Next week:** ML systems in practice — pipelines, deployment, and ethics/fairness.

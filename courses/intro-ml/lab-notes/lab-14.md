# Lab Notes 14 — Recommender Systems

**Concept recap:** content-based filtering uses item features; collaborative filtering uses
patterns in the ratings matrix itself; both commonly rely on cosine similarity.

**Common pitfalls:**
- Filling missing ratings with 0 and treating that as "a real rating of zero" rather than
  "unrated" — this can distort similarity calculations; consider mean-centering or only computing
  similarity over commonly-rated items.
- Expecting content-based and collaborative recommendations to always agree — they use entirely
  different signals and legitimately can diverge; that divergence is itself informative.
- Not excluding the target item/user itself when ranking "most similar" items/users (an item is
  trivially most similar to itself).

**Debugging tip:** if all similarity scores come out identical or zero, check that the
feature/ratings matrix wasn't accidentally transposed, and that rows/columns correspond to the
intended users/items.

**Instructor tip:** have students identify a user with very few ratings and discuss why
collaborative filtering struggles for that user (the "cold-start" problem for users), motivating
why a hybrid approach with content-based features can help.

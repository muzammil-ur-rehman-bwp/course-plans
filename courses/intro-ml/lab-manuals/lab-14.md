# Lab Manual 14 — Recommender Systems

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Build a content-based recommender and an item-based collaborative recommender, and compare their
outputs.

## Setup
Continue with scikit-learn/pandas; create `lab14.ipynb`.

## Procedure
1. **Task A — Content-based:** using the provided item-feature table, compute the item-item
   cosine similarity matrix; for a given item, list its top 5 most similar items.
2. **Task B — Collaborative (item-based):** using the provided user-item ratings matrix, compute
   item-item cosine similarity from the ratings themselves; for the same item as Task A, list its
   top 5 most similar items by this measure.
3. **Task C — Compare:** compare the two top-5 lists from Tasks A and B; discuss in 2–3 sentences
   why they differ (or agree).
4. **Task D — Predict a rating:** for one user and one item they haven't rated, predict a rating
   using a similarity-weighted average of their ratings for the most similar rated items.

## Expected Output
A notebook with Tasks A–D, including both similarity-based top-5 lists.

## Submission
Submit `lab14.ipynb` by the end of the lab session.

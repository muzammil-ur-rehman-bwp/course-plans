# Assignment 4 — Unsupervised Learning, Feature Engineering & Recommenders (Week 14)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 14 | **Due:** Start of Week 16

## Instructions
Submit `assignment04.ipynb` with working code and written answers for all questions, using the
provided dataset `assignment04_data.csv` (a mixed-type tabular dataset) and
`assignment04_ratings.csv` (a small user-item ratings matrix).

## Questions
1. **(Feature engineering, 20 pts)** Build a `ColumnTransformer`-based preprocessing pipeline for
   `assignment04_data.csv` (handle missing values, encode categoricals, scale numeric features);
   report the shape of the transformed feature matrix.
2. **(Clustering, 20 pts)** On the preprocessed features, fit `KMeans` for `k` in `2..8`; report
   the elbow plot and the silhouette score for each `k`, and state your chosen `k` with
   justification.
3. **(Dimensionality reduction, 20 pts)** Fit `PCA` on the same preprocessed features; report how
   many components are needed to explain at least 85% of the variance, and produce a 2D PCA
   scatter plot colored by your Question 2 cluster assignments.
4. **(Recommender, 20 pts)** Using `assignment04_ratings.csv`, compute item-item cosine
   similarity from the ratings matrix; for a specified item, report its top 5 most similar items.
5. **(Written reflection, 20 pts)** In 150–250 words, discuss one way the clustering from
   Question 2 and the recommender from Question 4 could be combined in a realistic system (e.g.,
   using clusters to recommend to new users with no rating history).

## Submission
Upload `assignment04.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

# Week 11 Summary — Feature Engineering & Preprocessing

**Key takeaways:**
- One-hot encoding suits nominal categories; ordinal encoding suits categories with a true order.
- Missing data is handled by imputation (mean/median/most-frequent) or, when missingness itself
  is informative, an explicit missingness indicator; imputers must be fit on training data only.
- Standardization, min-max, and robust scaling suit different data characteristics; tree-based
  models generally do not require scaling, while distance/gradient-based models do.
- `ColumnTransformer` applies different preprocessing to different columns within one pipeline;
  filter, wrapper, and embedded methods offer different approaches to feature selection.

**You should now be able to:** design and implement a `ColumnTransformer`-based preprocessing
pipeline for mixed-type, incomplete real-world data, and apply a feature selection technique
appropriate to the situation.

**Next week:** unsupervised learning I — k-means and hierarchical clustering.

# Lab Notes 11 — Feature Engineering & Preprocessing

**Concept recap:** different column types need different preprocessing; `ColumnTransformer`
bundles this per-column logic into one fittable, leak-safe pipeline.

**Common pitfalls:**
- Fitting the imputer or encoder on the full dataset (train + test) before splitting — leaks
  distributional information (e.g., the median used for imputation) from test data.
- One-hot encoding a high-cardinality nominal column (e.g., hundreds of distinct values) without
  considering the resulting column explosion, which can hurt both performance and runtime.
- Treating an ordinal feature as nominal (one-hot encoding it) and losing the ordering
  information a model could otherwise use directly.

**Debugging tip:** if `OneHotEncoder` raises an error on new/test data, check `handle_unknown`;
set it to `"ignore"` so categories unseen during training do not crash prediction.

**Instructor tip:** have students compare model performance with and without the missingness
indicator column on a feature where missingness correlates with the target, to see when that
extra signal actually helps.

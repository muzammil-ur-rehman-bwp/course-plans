# Lab Notes 5 — k-Nearest Neighbors and Naive Bayes

**Concept recap:** k-NN is distance-based and needs scaled features; Naive Bayes assumes
conditional feature independence given the class and does not require scaling.

**Common pitfalls:**
- Forgetting to scale features before k-NN — an unscaled feature with a large numeric range will
  dominate the distance calculation and distort neighbor selection.
- Choosing `k` by test-set accuracy directly instead of validation/cross-validation accuracy,
  which leaks test information into model selection.
- Applying `GaussianNB` to clearly non-Gaussian or count-based features (e.g., raw word counts) —
  `MultinomialNB` is the appropriate choice for count data.

**Debugging tip:** if k-NN accuracy barely changes across very different `k` values, check that
features were actually scaled — on unscaled data a single dominant feature can make `k` nearly
irrelevant.

**Instructor tip:** have students briefly inspect feature value ranges (`X.describe()`) before
scaling, so the motivation for scaling k-NN is seen in the data, not just asserted.

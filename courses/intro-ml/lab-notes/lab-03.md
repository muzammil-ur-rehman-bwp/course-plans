# Lab Notes 3 — Ridge, Lasso, and Elastic Net Regression

**Concept recap:** Ridge shrinks coefficients smoothly; Lasso can zero them out; Elastic Net mixes
both penalties. All three require feature scaling to penalize coefficients fairly.

**Common pitfalls:**
- Fitting `StandardScaler` outside/before the train/test split (or on the full dataset) — this
  leaks test-set statistics into preprocessing. Always scale inside a `Pipeline` fit only on
  training data.
- Expecting Lasso to always outperform Ridge — Lasso helps most when many features are truly
  irrelevant; with many weakly-informative, correlated features, Ridge or Elastic Net often
  generalizes better.
- Choosing `alpha` by eye from the coefficient-path plot alone instead of by validation
  performance — the "best-looking" path is not necessarily the best-performing model.

**Debugging tip:** if Lasso zeroes out *all* coefficients, `alpha` is too large; if it matches
plain linear regression exactly, `alpha` is too small (or effectively zero after scaling).

**Instructor tip:** have students report the number of non-zero Lasso coefficients as a function
of `alpha` as a simple table — the monotonic decrease makes the "automatic feature selection"
property concrete.

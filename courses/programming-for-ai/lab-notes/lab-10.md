# Lab Notes 10 — Regression

**Concept recap:** linear regression minimizes MSE; logistic regression outputs a probability
via sigmoid for binary classification; `StandardScaler` must be fit only on training data.

**Common pitfalls:**
- Fitting `StandardScaler` on the full dataset (train+test) before splitting — this leaks test
  information into preprocessing and gives overly optimistic results.
- Interpreting logistic regression coefficients as if they were linear regression coefficients
  (they relate to log-odds, not the raw probability, directly).

**Debugging tip:** if MSE seems unreasonably large, check that features aren't on wildly
different scales (e.g., one feature in the thousands, another between 0–1) — this disproportionately
affects models sensitive to feature scale.

**Instructor tip:** make the scaling pitfall concrete by having students compute results both the
"wrong" way (scale before split) and the "right" way (scale after split, fit on train only), and
compare — the difference illustrates the leakage risk directly.

# Presentation: Module 3 — SVMs, Evaluation & Feature Engineering (Weeks 9–11)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 3: SVMs, Evaluation & Feature Engineering
2. **Margin maximization** — maximum-margin hyperplane diagram with support vectors highlighted
3. **Soft margins** — the `C` hyperparameter's effect on margin width vs. training fit
4. **The kernel trick** — linear vs. polynomial vs. RBF decision-boundary plots, side by side
5. **k-fold cross-validation** — k-fold diagram (train/validation fold rotation)
6. **Nested cross-validation** — inner/outer loop diagram
7. **GridSearchCV & RandomizedSearchCV** — results table/heatmap over a hyperparameter grid
8. **Learning curves** — the three canonical shapes (good fit, high bias, high variance)
9. **Bias-variance tradeoff, quantitatively** — the error decomposition formula with the classic
   U-shaped curve
10. **Encoding categorical variables** — one-hot vs. ordinal encoding diagram
11. **Handling missing data** — imputation strategies summary table
12. **Feature scaling methods** — standardization vs. min-max vs. robust scaling comparison
13. **ColumnTransformer** — pipeline diagram showing per-column branches merging
14. **Feature selection** — filter/wrapper/embedded methods comparison table
15. **Module 3 recap**

**Speaker notes:** slides 5–9 (evaluation) are conceptually dense and benefit from a live
`GridSearchCV`/learning-curve demo immediately after, rather than being left purely abstract.

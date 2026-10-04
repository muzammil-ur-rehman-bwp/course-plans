# Lab Notes 9 — scikit-learn Setup

**Concept recap:** `train_test_split(X, y, test_size=0.2, random_state=42)` randomly partitions
data while keeping a fixed seed for reproducibility; the test set must stay untouched until final
evaluation.

**Common pitfalls:**
- Forgetting `random_state`, making results non-reproducible across runs.
- Accidentally including the target column in the feature matrix `X`.

**Instructor tip:** this is a deliberately light lab given the midterm — use the extra time for
individual office-hours-style help on anything from Weeks 1–8 students are still shaky on.

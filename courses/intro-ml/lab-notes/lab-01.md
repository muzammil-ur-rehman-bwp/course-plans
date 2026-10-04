# Lab Notes 1 — Environment Setup & Dataset Exploration

**Concept recap:** the ML workflow starts with loading and understanding data before any
modeling; `train_test_split` creates a held-out test set that must not be used again until final
evaluation.

**Common pitfalls:**
- Not setting `random_state`, which makes results non-reproducible across runs.
- Looking at summary statistics or plots of the *test* set before modeling and letting that
  shape modeling decisions — this is a mild form of information leakage, since decisions should
  be based only on training data.
- Confusing `.shape` on a DataFrame vs. a Series when checking split sizes.

**Debugging tip:** if `pd.read_csv` raises a parsing error, check for an unexpected delimiter or a
header row mismatch — print the first few raw lines of the file before loading.

**Instructor tip:** have students compute the train and test set sizes as percentages and confirm
they sum to 100%, to build the habit of sanity-checking splits throughout the semester.

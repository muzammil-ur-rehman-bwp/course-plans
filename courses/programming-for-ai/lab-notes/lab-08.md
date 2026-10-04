# Lab Notes 8 — Naive Bayes

**Concept recap:** Naive Bayes assumes conditional independence of features (words) given the
class; Laplace (add-one) smoothing prevents zero probabilities for unseen words; predictions are
made by comparing `log`-probabilities across classes (logs avoid numerical underflow from
multiplying many small probabilities).

**Common pitfalls:**
- Forgetting smoothing, causing any unseen word to zero out an entire class's probability.
- Multiplying raw probabilities instead of summing log-probabilities, risking numerical
  underflow for longer documents.
- Not separating train/test data, leading to inflated accuracy claims.

**Debugging tip:** print the top 5 words by `word_counts[class]` for each class — this is a
quick sanity check that the model learned something sensible (e.g., "free", "win" for spam).

**Instructor tip:** this is students' first classifier evaluated purely "by hand" — use it to
reinforce *why* Week 9 onward will switch to scikit-learn's tested, optimized implementations
rather than hand-rolled code for every model.

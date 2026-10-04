# Lab Notes 14 — NLP Survey: Tokenization & Bag-of-Words

**Concept recap:** tokenization splits raw text into discrete units; bag-of-words represents a
document as a word-count vector against a shared vocabulary, discarding word order entirely —
useful for simple tasks, but blind to meaning that depends on order (e.g., negation, who did
what to whom).

**Common pitfalls:**
- Building a different vocabulary per document instead of one shared vocabulary across all
  documents being compared — vectors must have the same length and dimension-ordering to be
  comparable at all.
- Forgetting to lowercase and strip punctuation consistently, which silently creates duplicate
  vocabulary entries (`"Dog"` and `"dog."` as different tokens).
- Treating a high word-overlap score as "the classifier is confident" rather than what it
  actually measures — raw overlap counts are not a calibrated probability.

**Debugging tip:** print the vocabulary list alongside each document's vector so you can read
off which words contributed to a count directly — this makes it obvious when tokenization is
splitting words unexpectedly (e.g., contractions breaking into two bogus tokens).

**Instructor tip:** the Task D "identical vectors, different meaning" exercise lands best when
students construct their *own* pair of sentences rather than using an instructor-provided one —
having to find a convincing pair themselves builds the intuition the exercise is meant to teach.

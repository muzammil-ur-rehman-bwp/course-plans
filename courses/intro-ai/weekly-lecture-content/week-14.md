# Week 14 — Lecture Content: Natural Language Processing Overview

**Scope note:** a survey week, like Weeks 12–13. The goal is to understand why language is hard
for AI and to see one simple, concrete representation (bag-of-words) — not to build a production
NLP pipeline.

## 1. Why Natural Language Is Hard for AI
Unlike the clean, formal syntax of Weeks 6–8's logic, natural language is:
- **Ambiguous** lexically ("bank" the financial institution vs. the river bank), syntactically
  ("I saw the man with the telescope" — who has the telescope?), and semantically.
- **Context-dependent**: meaning often depends on the surrounding sentences or situation
  ("it" needs a referent).
- **Reliant on world knowledge**: "The city council denied the demonstrators a permit because
  they feared violence" requires knowing who plausibly fears violence to resolve "they."

These are the same kinds of representation challenges search and logic were built to handle
(Weeks 3–8), but language adds that the "rules" themselves are fuzzy, numerous, and full of
exceptions — which is a large part of why statistical/learned approaches (Weeks 12–13) have
become dominant in NLP specifically.

## 2. Tokenization
The first step in almost any NLP pipeline: splitting raw text into discrete units (tokens),
typically words or punctuation marks.

```python
import re

def tokenize(text):
    text = text.lower()
    return re.findall(r"[a-z0-9]+", text)

print(tokenize("The quick brown fox jumps over the lazy dog!"))
# ['the', 'quick', 'brown', 'fox', 'jumps', 'over', 'the', 'lazy', 'dog']
```
Real tokenizers must handle contractions, hyphenation, numbers, and non-English scripts far more
carefully than this; the simple regex above is sufficient for illustrating the idea.

## 3. The Bag-of-Words Representation
Represent a document as a vector of word counts, ignoring word order entirely.

```python
from collections import Counter

def bag_of_words(text, vocabulary):
    counts = Counter(tokenize(text))
    return [counts.get(word, 0) for word in vocabulary]

documents = [
    "the dog barked at the cat",
    "the cat sat on the mat",
]
vocabulary = sorted(set(word for doc in documents for word in tokenize(doc)))
vectors = [bag_of_words(doc, vocabulary) for doc in documents]
print("vocabulary:", vocabulary)
for v in vectors:
    print(v)
```

## 4. A Toy Text-Classification Demo
With bag-of-words vectors, even a trivially simple classifier (e.g., comparing word-overlap
counts, or a hand-rolled Naive-Bayes-style scoring) can distinguish categories of short texts.

```python
def word_overlap_score(doc_vector, class_vector):
    return sum(d * c for d, c in zip(doc_vector, class_vector))

sports_vocab_vector = bag_of_words("the team scored a goal in the match", vocabulary)
new_doc = "the cat sat near the goal post"
new_vector = bag_of_words(new_doc, vocabulary)
print("overlap score with 'sports-like' words:",
      word_overlap_score(new_vector, sports_vocab_vector))
```

## 5. What Bag-of-Words Loses
Because word order is discarded, "the dog bit the man" and "the man bit the dog" have the
*identical* bag-of-words vector, despite opposite meanings. This is a deliberate, honest
limitation to highlight: it motivates (without this course implementing them) n-grams, word
order-aware models, and ultimately the attention-based large language models that dominate
current NLP — a brief, survey-level pointer to where the field has gone, consistent with this
course's classical/conceptual scope.

## 6. In-Class Exercise
Tokenize two short sentences by hand, build their bag-of-words vectors against a shared
vocabulary, and identify one piece of information the representation loses for each (negation,
word order, or sarcasm are good discussion starters).

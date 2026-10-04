# Lab Manual 14 — NLP Survey: Tokenization & Bag-of-Words

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Implement a simple tokenizer and bag-of-words vectorizer from scratch; use it for a toy
text-classification demo.

## Setup
Create `lab14.ipynb`; use the `tokenize` and `bag_of_words` functions from the lecture content
as a starting point.

## Procedure
1. **Task A — Tokenizer:** implement/paste `tokenize(text)`; test it on 3 sentences including
   punctuation and mixed case, and confirm tokens are lowercase with punctuation removed.
2. **Task B — Bag-of-words:** implement/paste `bag_of_words(text, vocabulary)`; build a shared
   vocabulary from a provided set of 6–8 short documents and compute each document's vector.
3. **Task C — Toy classification:** given two small labeled groups of documents (e.g., "sports"
   vs. "cooking" sentences, provided by the instructor), compute a simple word-overlap score
   between a new, unlabeled document and each group's combined bag-of-words vector; classify the
   new document by the higher-scoring group.
4. **Task D — Limitation demo:** construct two sentences with identical bag-of-words vectors but
   different meaning (e.g., swapping subject and object), compute both vectors to confirm they
   are identical, and write 2–3 sentences explaining the limitation this demonstrates.

## Expected Output
A notebook with Tasks A–D; Task D must include two sentences whose bag-of-words vectors are
verified (by code) to be exactly equal despite different meaning.

## Submission
Submit `lab14.ipynb` by the end of the lab session.

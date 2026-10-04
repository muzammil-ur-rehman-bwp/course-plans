# Lab Manual 11 — A Linear-Chain CRF for Sequence Labeling

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Fit a linear-chain Conditional Random Field to a small sequence-labeling task and inspect what it
learns.

## Setup
Create `lab11.ipynb`. `pip install sklearn-crfsuite`.

## Procedure
1. **Task A — Feature functions:** implement `word_features`/`sent_features` as in the lecture
   content, and extend them with at least one additional feature of your choosing (e.g., "is a
   digit," "contains a hyphen").
2. **Task B — Train/predict:** train the CRF on a provided small tagged corpus; predict tags for
   a held-out sentence and print the result.
3. **Task C — Feature inspection:** print the top 10 highest-magnitude learned state features and
   top 10 transition features; discuss whether they make linguistic sense.
4. **Task D — Generative baseline:** implement (or use a provided) simple first-order HMM with
   counted transition/emission probabilities on the same corpus; compare its tagging accuracy to
   the CRF's.
5. **Task E — Reflection:** explain, in 3–4 sentences and referencing Task A's added feature,
   why that specific feature would be awkward to incorporate into the HMM's emission model but
   was easy to add to the CRF.

## Expected Output
A notebook with Tasks A–E, including the printed feature lists and the HMM/CRF accuracy
comparison.

## Submission
Submit `lab11.ipynb` by the end of the lab session.

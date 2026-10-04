# Lab Manual 8 — Naive Bayes Classifier

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement a Naive Bayes text classifier from scratch and evaluate it on a small labeled dataset.

## Setup
Create `lab08.ipynb`; use the instructor-provided small labeled text dataset (e.g., spam/ham
messages).

## Procedure
1. **Task A — Implementation:** implement `NaiveBayesTextClassifier` (fit/predict) as shown in
   the lecture, including Laplace smoothing.
2. **Task B — Training & prediction:** fit on the provided training split; predict labels for
   the test split.
3. **Task C — Evaluation:** compute accuracy on the test split by hand (without scikit-learn).
4. **Task D — Reflection:** identify one example the classifier got wrong and explain, using the
   model's computed scores, why it made that mistake.

## Expected Output
A notebook with Tasks A–D; Task D must reference actual printed scores/probabilities, not just a
general statement.

## Submission
Submit `lab08.ipynb` by the end of the lab session.

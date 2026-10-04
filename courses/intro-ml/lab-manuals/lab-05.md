# Lab Manual 5 — k-Nearest Neighbors and Naive Bayes

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Fit and tune a k-NN classifier, fit Naive Bayes variants, and compare their decision boundaries.

## Setup
Continue with scikit-learn; create `lab05.ipynb`.

## Procedure
1. **Task A — k-NN tuning:** using the provided 2D dataset, fit `KNeighborsClassifier` (inside a
   scaled pipeline) for `k` in `{1, 5, 15, 25}`; report validation accuracy for each via
   `cross_val_score` and identify the best `k`.
2. **Task B — Naive Bayes:** fit `GaussianNB` on the same dataset; report test accuracy and
   compare to the best k-NN model.
3. **Task C — Text classification:** fit `MultinomialNB` on the provided text dataset (already
   vectorized as word counts); report test accuracy.
4. **Task D — Decision boundaries:** plot decision boundaries for the best k-NN model and
   `GaussianNB` on the 2D dataset; describe the visible difference in one or two sentences.

## Expected Output
A notebook with Tasks A–D, including two decision-boundary plots.

## Submission
Submit `lab05.ipynb` by the end of the lab session.

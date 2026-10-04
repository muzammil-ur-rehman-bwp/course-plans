# Lab Manual 11 — Feature Engineering & Preprocessing

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Build a `ColumnTransformer`-based preprocessing pipeline for a mixed-type dataset with missing
values, and apply a feature selection technique.

## Setup
Continue with scikit-learn/pandas; create `lab11.ipynb`.

## Procedure
1. **Task A — Inspect:** load the provided dataset; identify numeric, nominal categorical, and
   ordinal categorical columns, and report the percentage of missing values per column.
2. **Task B — Build the pipeline:** construct a `ColumnTransformer` with a numeric sub-pipeline
   (median imputation + `StandardScaler`) and a categorical sub-pipeline (most-frequent
   imputation + `OneHotEncoder`); wrap it with a `LogisticRegression` in a full `Pipeline`.
3. **Task C — Fit and evaluate:** fit the full pipeline on the training set; report test
   accuracy and F1-score.
4. **Task D — Feature selection:** apply `SelectKBest` (or `RFE`) on top of the preprocessed
   features; report test performance with the top 10 features only, and compare to Task C.

## Expected Output
A notebook with Tasks A–D, including a short written comparison for Task D.

## Submission
Submit `lab11.ipynb` by the end of the lab session.

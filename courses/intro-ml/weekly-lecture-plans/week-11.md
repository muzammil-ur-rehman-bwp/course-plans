# Week 11 Lecture Plan — Introduction to Machine Learning
## Topic: Feature Engineering & Preprocessing

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain common encoding, imputation, and scaling techniques for real-world tabular data.
   (*Understand*)
2. Apply `OneHotEncoder`, `SimpleImputer`, scalers, and a `ColumnTransformer` to mixed-type data.
   (*Apply*)
3. Analyze feature selection techniques and choose one appropriate to a given dataset. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Encoding categorical variables | One-hot vs. ordinal encoding; when each is appropriate |
| 0:25–0:45 | Handling missing data | Imputation strategies (mean/median/most-frequent, indicator flags) |
| 0:45–0:55 | Break | — |
| 0:55–1:15 | Feature scaling methods | Standardization, min-max, robust scaling; which models need which |
| 1:15–1:40 | `ColumnTransformer` | Applying different preprocessing to different columns in one object |
| 1:40–2:00 | Feature selection | Filter (correlation/univariate), wrapper (RFE), embedded (Lasso/tree importance) methods |

### Materials/Equipment
- Live-coding environment, scikit-learn, pandas; a dataset with mixed numeric/categorical
  features and some missing values.

### Formative Check (in-class)
Exercise: for a given dataset's columns, decide whether each should be one-hot encoded, ordinal
encoded, scaled, or left as-is, and justify each choice.

### Link to Lab/Assessment
Lab 11: feature engineering lab building a `ColumnTransformer` pipeline for mixed-type data with
missing values.

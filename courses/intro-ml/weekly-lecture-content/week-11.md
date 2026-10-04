# Week 11 — Lecture Content: Feature Engineering & Preprocessing

## 1. Encoding Categorical Variables
Most scikit-learn models require numeric input, so categorical features must be encoded.
- **One-hot encoding:** creates one binary column per category; appropriate for **nominal**
  categories with no inherent order (e.g., city, color). Avoids implying a false ordering.
- **Ordinal encoding:** maps categories to integers; appropriate for **ordinal** categories with
  a true order (e.g., "low"/"medium"/"high"), since the integer order is meaningful to some
  models.

```python
from sklearn.preprocessing import OneHotEncoder, OrdinalEncoder

ohe = OneHotEncoder(handle_unknown="ignore", sparse_output=False)
X_city_encoded = ohe.fit_transform(X_train[["city"]])

ordinal = OrdinalEncoder(categories=[["low", "medium", "high"]])
X_level_encoded = ordinal.fit_transform(X_train[["level"]])
```
Using one-hot encoding on a nominal feature but feeding it to a model as if the categories were
ordered (or vice versa) is a common source of silently poor performance.

## 2. Handling Missing Data
Real data frequently has missing values. Common strategies:
- **Mean/median imputation** for numeric features (median is more robust to outliers).
- **Most-frequent (mode) imputation** for categorical features.
- **Missingness indicator:** add a binary column flagging whether a value was originally missing,
  when "missingness" itself may carry information (e.g., a skipped survey question).

```python
from sklearn.impute import SimpleImputer

num_imputer = SimpleImputer(strategy="median")
X_train_num_imputed = num_imputer.fit_transform(X_train[numeric_cols])

cat_imputer = SimpleImputer(strategy="most_frequent")
X_train_cat_imputed = cat_imputer.fit_transform(X_train[categorical_cols])
```
As always, imputers must be **fit on training data only**, then applied to validation/test data,
to avoid leaking information about the full dataset's distribution into preprocessing.

## 3. Feature Scaling Methods
- **Standardization (`StandardScaler`):** `(x - mean) / std`; assumes roughly Gaussian-ish data;
  the default choice for linear models, SVMs, and k-NN.
- **Min-max scaling (`MinMaxScaler`):** rescales to `[0, 1]`; useful when a bounded range is
  wanted (e.g., for some neural network inputs) but sensitive to outliers.
- **Robust scaling (`RobustScaler`):** uses the median and interquartile range instead of mean and
  standard deviation; preferred when a feature has substantial outliers.

```python
from sklearn.preprocessing import StandardScaler, RobustScaler

scaler = RobustScaler()
X_train_scaled = scaler.fit_transform(X_train[numeric_cols])
```
Tree-based models (decision trees, Random Forests, boosting) are invariant to monotonic feature
scaling and generally do **not** require it; distance- and gradient-based models (k-NN, SVMs,
linear/logistic regression, regularized regression) generally do.

## 4. ColumnTransformer: Different Preprocessing for Different Columns
Real datasets mix numeric and categorical columns needing different treatment.
`ColumnTransformer` applies a different pipeline to each specified set of columns and
concatenates the results — and, wrapped in an outer `Pipeline`, is fit only on training data:
```python
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline

numeric_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])
categorical_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("encoder", OneHotEncoder(handle_unknown="ignore")),
])

preprocessor = ColumnTransformer([
    ("num", numeric_pipeline, numeric_cols),
    ("cat", categorical_pipeline, categorical_cols),
])
full_pipeline = Pipeline([("preprocess", preprocessor), ("model", LogisticRegression(max_iter=1000))])
full_pipeline.fit(X_train, y_train)
```

## 5. Feature Selection Techniques
- **Filter methods:** rank features independently of any model, e.g., correlation with the
  target or a univariate statistical test (`SelectKBest` with `f_classif`/`chi2`).
- **Wrapper methods:** search over feature subsets using a model's actual performance as the
  criterion, e.g., **Recursive Feature Elimination (RFE)**, which repeatedly fits a model and
  drops the least important feature(s).
- **Embedded methods:** feature selection happens as a side effect of training, e.g., Lasso's
  coefficients going to exactly zero (Week 3), or a tree ensemble's feature importances
  (Week 7) used to drop low-importance features.

```python
from sklearn.feature_selection import SelectKBest, f_classif, RFE

selector = SelectKBest(score_func=f_classif, k=10)
X_train_selected = selector.fit_transform(X_train_scaled, y_train)

rfe = RFE(estimator=LogisticRegression(max_iter=1000), n_features_to_select=10)
rfe.fit(X_train_scaled, y_train)
```

## 6. In-Class Exercise
Given a small dataset's column list (a mix of numeric, nominal categorical, ordinal categorical,
and a column with 15% missing values), design the preprocessing plan for each column and justify
each choice in one sentence.

# Week 4 — Lecture Content: pandas & Exploratory Data Analysis

## 1. Series and DataFrame
```python
import pandas as pd

s = pd.Series([10, 20, 30], name="score")
df = pd.read_csv("data.csv")
df.head()
df.info()
df.describe()
```
A `DataFrame` is a table: rows are observations, columns are variables, each column is a `Series`.

## 2. Indexing & Filtering
```python
df.loc[0, "score"]          # label-based indexing
df.iloc[0, 1]                # position-based indexing
df[df["score"] > 70]         # boolean mask filtering
df[["name", "score"]]        # column selection
```

## 3. Grouping, Aggregation, Merging
```python
df.groupby("class")["score"].mean()
pd.merge(df_students, df_grades, on="student_id", how="left")
df.isna().sum()              # count missing values per column
df.fillna(df.mean(numeric_only=True))   # simple imputation
```

## 4. Exploratory Data Analysis (EDA) Workflow
1. Load the data (`read_csv`/`read_json`) and inspect shape/dtypes (`info()`, `describe()`).
2. Identify and handle missing values and obvious data-quality issues (duplicates, outliers).
3. Explore distributions and relationships: histograms, scatter plots, group-wise means.
4. Summarize findings before moving to modeling — EDA is not optional, it drives every modeling
   decision later in the course (feature choice, scaling, which model family is appropriate).

```python
import matplotlib.pyplot as plt

df["score"].hist(bins=20)
plt.xlabel("score"); plt.ylabel("count"); plt.title("Score distribution")
plt.show()
```

## 5. In-Class Exercise
On the provided dataset: report the number of missing values per column, impute or drop them
with a justified choice, and produce one plot that reveals a relationship between two variables.

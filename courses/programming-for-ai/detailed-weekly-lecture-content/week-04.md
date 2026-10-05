# Week 4: pandas and Exploratory Data Analysis

## Learning Objectives

By the end of this lecture, you should be able to:

1. Create and inspect a pandas Series and DataFrame.
2. Select data with `loc`, `iloc`, boolean masks and column lists, and know the difference between them.
3. Group, aggregate and merge tables.
4. Detect and handle missing values and duplicates with a defensible choice.
5. Carry out a short exploratory data analysis (EDA) and support your findings with a plot.

## 1. Series and DataFrame

NumPy is excellent for numbers, but real data comes with column names, mixed types, missing entries and text. pandas adds labelled structure on top of NumPy arrays.

A Series is a one-dimensional labelled array. A DataFrame is a table whose columns are Series sharing a common index. Rows are observations and columns are variables.

```python
import pandas as pd

s = pd.Series([10, 20, 30], name="score")
print(s)
print(s.mean(), s.dtype)
```

Throughout this lecture we need a dataset, and we should not depend on a file you may not have. The following code builds a small, slightly messy student dataset and saves it as `data.csv`. Run it first. It gives us realistic problems: missing values, a duplicated row and one suspicious outlier.

```python
import numpy as np
import pandas as pd

rng = np.random.default_rng(10)
n = 60

hours = rng.uniform(0, 12, size=n).round(1)
attendance = rng.uniform(50, 100, size=n).round(0)
score = (30 + 4.5 * hours + 0.2 * attendance + rng.normal(0, 6, size=n)).clip(0, 100).round(0)

df_raw = pd.DataFrame({
    "student_id": range(1, n + 1),
    "name": [f"student_{i}" for i in range(1, n + 1)],
    "class": rng.choice(["A", "B", "C"], size=n),
    "hours_studied": hours,
    "attendance": attendance,
    "score": score,
})

# add typical data quality problems
df_raw.loc[[4, 17, 33], "attendance"] = np.nan
df_raw.loc[[8, 41], "score"] = np.nan
df_raw.loc[25, "hours_studied"] = 120.0            # impossible value
df_raw = pd.concat([df_raw, df_raw.iloc[[10]]])     # duplicate row

df_raw.to_csv("data.csv", index=False)
print(df_raw.shape)
```

Now load it, as you would any CSV file.

```python
df = pd.read_csv("data.csv")
print(df.head())
df.info()
print(df.describe())
```

What each call tells you:

1. `head()` shows the first five rows. Always look at real rows before doing anything else.
2. `info()` lists columns, types and the count of non-null values. A column with fewer non-null values than rows has missing data.
3. `describe()` gives count, mean, standard deviation, quartiles, minimum and maximum for numeric columns. Look at the `max` of `hours_studied`. It should stand out.

## 2. Indexing and Filtering

pandas has several ways to select data. The mistakes usually come from mixing them up.

```python
print(df.loc[0, "score"])           # label-based: row label 0, column "score"
print(df.iloc[0, 5])                # position-based: first row, sixth column
print(df[["name", "score"]].head()) # select columns with a list
print(df["score"].head())           # a single column gives a Series
```

1. `loc` uses labels. In a freshly loaded table the labels happen to be 0, 1, 2 and so on.
2. `iloc` uses integer positions, like NumPy.
3. A list of column names returns a DataFrame. A single name returns a Series.

### 2.1 Boolean masks

```python
high = df[df["score"] > 70]
print(len(high), "students scored above 70")

mask = (df["class"] == "A") & (df["attendance"] > 80)
print(df[mask][["name", "class", "attendance", "score"]])

print(df[df["class"].isin(["A", "B"])].shape)
```

Use `&` and `|` with parentheses around each condition, not the words `and` and `or`. The words do not work on whole columns.

### 2.2 Creating and changing columns

```python
df["passed"] = df["score"] >= 50
df["study_per_attend"] = df["hours_studied"] / df["attendance"]
print(df[["score", "passed", "study_per_attend"]].head())

df = df.rename(columns={"hours_studied": "hours"})
print(df.columns.tolist())
```

## 3. Grouping, Aggregation and Merging

### 3.1 Group-wise summaries

```python
print(df.groupby("class")["score"].mean())

summary = df.groupby("class").agg(
    students=("student_id", "count"),
    mean_score=("score", "mean"),
    max_hours=("hours", "max"),
)
print(summary.round(2))
```

The pattern is split, apply, combine. pandas splits the table by class, applies a function to each part, and combines the answers. Group-wise comparison is one of the quickest ways to see whether a variable matters.

### 3.2 Merging tables

Data is often spread over several tables. Suppose a second table holds each student's grade on a coursework item.

```python
df_students = df[["student_id", "name"]].drop_duplicates()
df_grades = pd.DataFrame({
    "student_id": [1, 2, 3, 4, 999],
    "coursework": [78, 85, 62, 90, 55],
})

merged = pd.merge(df_students, df_grades, on="student_id", how="left")
print(merged.head(6))
```

With `how="left"` every row from the left table is kept. Students without coursework get `NaN`, and student 999 is dropped because the left table has no such student. Try `how="inner"`, `"right"` and `"outer"` and see what changes. Choosing the wrong join type quietly loses or invents rows, so check the row count after every merge.

### 3.3 Missing values

```python
print(df.isna().sum())               # missing values per column
print(df.duplicated().sum())         # fully duplicated rows
```

There are several honest ways to handle missing data, and the right choice depends on the situation.

1. Drop rows with missing values. Simple, but you lose data, and if values are not missing at random you may bias the result.
2. Fill with a statistic such as the mean or median. Easy, but it shrinks the variation in that column.
3. Fill using a group-wise statistic, for example the mean of the student's class.
4. Keep the missing value and let a model that supports it deal with it.

```python
df = df.drop_duplicates()

# median is less sensitive to extreme values than the mean
df["attendance"] = df["attendance"].fillna(df["attendance"].median())

# a missing score is our target variable, so we do not invent it
df = df.dropna(subset=["score"])

print(df.isna().sum())
print(df.shape)
```

The reasoning is the important part. We filled attendance, which is a feature, with the median. We dropped rows with a missing score, because that is the value we would later try to predict, and inventing target values would be a form of cheating.

### 3.4 Outliers

Earlier `describe()` showed a study time of 120 hours. That cannot be right for one student in a week.

```python
print(df.sort_values("hours", ascending=False).head(3)[["name", "hours", "score"]])

q1, q3 = df["hours"].quantile([0.25, 0.75])
iqr = q3 - q1
upper = q3 + 1.5 * iqr
print("upper fence:", round(upper, 2))
print(df[df["hours"] > upper][["name", "hours"]])

df = df[df["hours"] <= upper]
print(df.shape)
```

The 1.5 times IQR rule is a common starting point, not a law. Before removing a point, ask whether it is an error, as it is here, or a rare but real observation that carries information.

## 4. Exploratory Data Analysis Workflow

EDA is the disciplined habit of looking at data before modelling it. It is not optional, because it drives every later decision: which features to use, how to scale them, and what kind of model is plausible.

A reasonable workflow is as follows.

1. Load the data and check shape and types with `info()` and `describe()`.
2. Find and handle missing values, duplicates and impossible values.
3. Explore single variables: distributions, ranges and categories.
4. Explore relationships between variables: scatter plots, correlations and group-wise means.
5. Write down what you learned, in words, before moving on.

### 4.1 Single variables

```python
import matplotlib.pyplot as plt

df["score"].hist(bins=15)
plt.xlabel("score")
plt.ylabel("count")
plt.title("Score distribution")
plt.show()

print(df["class"].value_counts())
```

Read a histogram for its shape. Is it symmetric, skewed or does it have two humps? A two-humped distribution often hides two different groups.

### 4.2 Relationships

```python
plt.scatter(df["hours"], df["score"])
plt.xlabel("hours studied per week")
plt.ylabel("score")
plt.title("Score against study time")
plt.show()

print(df[["hours", "attendance", "score"]].corr().round(2))
```

The scatter plot should show a rising pattern, and the correlation matrix puts a number on it. Correlation measures linear association only, and it says nothing about cause. Students who study more may also be more motivated in ways the data does not capture.

### 4.3 Group comparison

```python
df.boxplot(column="score", by="class")
plt.suptitle("")
plt.title("Score by class")
plt.ylabel("score")
plt.show()
```

In our synthetic data the classes were assigned at random, so the boxes should look alike. Seeing no difference is also a finding, and it tells you that class membership will probably not help a model.

## 5. Worked Example: A Mini EDA Report

Put the steps together into one function that prints a short report. This is a useful template for any new dataset.

```python
def quick_report(frame, target):
    print("Shape:", frame.shape)
    print("\nMissing values:")
    print(frame.isna().sum()[frame.isna().sum() > 0])
    print("\nDuplicate rows:", frame.duplicated().sum())
    num = frame.select_dtypes("number")
    print("\nCorrelation with", target)
    print(num.corr()[target].drop(target).sort_values(ascending=False).round(2))

fresh = pd.read_csv("data.csv")
quick_report(fresh, "score")          # raw file: the outlier hides the real pattern

cleaned = fresh.drop_duplicates()
cleaned = cleaned[cleaned["hours_studied"] < 24]
quick_report(cleaned, "score")        # after cleaning
```

Compare the two reports. On the raw file the correlation between hours studied and score looks weak, only about 0.13, because a single impossible value of 120 hours distorts it. After cleaning it rises above 0.9. This is a good reminder that one bad value can mislead an entire analysis. The expected finding is that hours studied has by far the strongest relationship with score, and attendance a weak one. A short written note might read: "Score rises with study time. About five percent of attendance values were missing and were filled with the median. One impossible study time was removed. Class does not appear to matter."

## 6. In-Class Exercise

On the dataset you are given (or `data.csv` from above):

1. Report the number of missing values per column.
2. Impute or drop them, and write one sentence justifying your choice for each column.
3. Produce one plot that reveals a relationship between two variables, and describe it in two sentences.

A model answer for the second part is the code in section 3.3. For the third part, try this variation that colours points by class.

```python
fresh = pd.read_csv("data.csv").drop_duplicates().dropna(subset=["score"])
fresh = fresh[fresh["hours_studied"] < 24]

for label, part in fresh.groupby("class"):
    plt.scatter(part["hours_studied"], part["score"], label=f"class {label}")
plt.legend()
plt.xlabel("hours studied")
plt.ylabel("score")
plt.show()
```

## 7. Common Mistakes

1. Chained indexing such as `df[df.x > 1]["y"] = 0`, which may not change the original table. Use `df.loc[df.x > 1, "y"] = 0`.
2. Using `and` instead of `&` in a filter.
3. Filling missing values before the train and test split, which leaks information from the test set. We return to this in Week 9.
4. Treating correlation as proof of cause.
5. Removing outliers automatically without looking at them.
6. Not checking row counts after a merge.

## 8. Summary

pandas organizes data as labelled tables and makes selection, grouping and merging concise. Most of the time in a data project goes on cleaning, and the choices made there, such as how to treat missing values, should be justified and recorded. EDA is the process of looking at the data from several angles before any model is built, and it will keep you from many avoidable mistakes in later weeks.

## 9. Practice Problems

1. Using `data.csv`, find the class with the highest average score, and the class whose scores vary the most (standard deviation).
2. Create a new column `attendance_band` with values "low", "medium" and "high" using `pd.cut`, then compare average scores across the bands.
3. Fill missing attendance with the mean of the student's own class using `groupby` and `transform`, and compare the result with filling by the overall median.
4. Write a function that, given any DataFrame, returns the names of columns with more than 20 percent missing values.

## 10. Suggested Reading

1. The pandas "10 minutes to pandas" guide.
2. The pandas user guide sections on indexing and on missing data.

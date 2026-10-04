# Lab Notes 4 — pandas & EDA

**Concept recap:** `DataFrame.info()`/`describe()` give a fast first look at a dataset;
`isna().sum()` quantifies missing data; `groupby` + an aggregation function summarizes data by
category; EDA should always precede modeling decisions.

**Common pitfalls:**
- Dropping rows/columns with missing data without checking how much data would be lost.
- Confusing `df.loc[]` (label-based) with `df.iloc[]` (position-based) indexing.
- Drawing a conclusion from a single plot without checking sample sizes per group (a group with
  3 data points can look dramatically different from one with 3,000 purely by chance).

**Debugging tip:** `df.dtypes` quickly reveals when a numeric column was accidentally loaded as
a string (common with CSVs containing stray characters or inconsistent formatting).

**Instructor tip:** push students to justify *why* they chose to impute vs. drop missing data in
Task B — this is a judgment call with no single right answer, and the reasoning matters more
than the specific choice.

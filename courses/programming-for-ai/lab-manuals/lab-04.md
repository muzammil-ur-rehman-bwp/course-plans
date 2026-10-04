# Lab Manual 4 — pandas & Exploratory Data Analysis

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Load, clean, explore, and visualize a real tabular dataset end-to-end using pandas.

## Setup
`pip install pandas matplotlib`; create `lab04.ipynb`; download the provided dataset CSV
(instructor-supplied, e.g., a public dataset such as Titanic or a housing dataset).

## Procedure
1. **Task A — Load & inspect:** load the CSV; report shape, dtypes, and `describe()` output.
2. **Task B — Missing data:** report missing values per column; impute or drop each column with
   a one-sentence justification per decision.
3. **Task C — Grouping:** compute a group-wise aggregate relevant to the dataset (e.g., mean
   value by category) using `groupby`.
4. **Task D — Visualization:** produce two plots: one distribution (histogram) and one
   relationship (scatter plot or grouped bar chart), each with axis labels and a title.
5. **Task E — EDA summary:** write a 3–5 sentence summary of the most interesting pattern found.

## Expected Output
A notebook with Tasks A–E; the EDA summary in Task E must reference specific numbers/plots
produced earlier in the notebook.

## Submission
Submit `lab04.ipynb` by the end of the lab session.

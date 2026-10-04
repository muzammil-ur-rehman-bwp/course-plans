# Lab Manual 1 — Environment Setup & Dataset Exploration

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Set up the Python ML toolchain, explore a dataset, and perform a correct train/test split.

## Setup
Install/verify Python 3.10+, NumPy, pandas, Matplotlib, and scikit-learn (via `pip install numpy
pandas matplotlib scikit-learn` or a provided Colab environment). Create `lab01.ipynb`.

## Procedure
1. **Task A — Environment check:** import `numpy`, `pandas`, `matplotlib.pyplot`, and `sklearn`;
   print each library's version.
2. **Task B — Load and inspect:** load the provided dataset (`housing.csv`) with pandas; print
   `.shape`, `.info()`, `.describe()`, and the first 5 rows.
3. **Task C — Basic visualization:** plot a histogram of the target variable and a scatter plot
   of one numeric feature against the target.
4. **Task D — Split:** perform an 80/20 train/test split with `train_test_split(random_state=42)`;
   print the shapes of the resulting arrays and confirm the split proportions.

## Expected Output
A notebook with Tasks A–D, including printed output and two plots.

## Submission
Submit `lab01.ipynb` by the end of the lab session.

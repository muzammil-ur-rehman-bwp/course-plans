# Assignment 1 — Python, NumPy, pandas (Weeks 1–4)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 4 | **Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + brief explanation) for each question.

## Questions
1. **(Python, 15 pts)** Write a function `word_frequencies(text: str) -> dict` that returns a
   dictionary mapping each word (lowercased, punctuation stripped) to its frequency count in
   `text`. Do not use any external library beyond the standard library.
2. **(NumPy, 20 pts)** Given a 2D NumPy array representing grayscale pixel values (0–255),
   write a vectorized function `normalize(img)` that rescales values to the range [0, 1]. Then
   write a second version using an explicit nested loop, and report the timing difference on a
   500x500 random array.
3. **(NumPy, 15 pts)** Implement `cosine_similarity(a, b)` for two 1D NumPy vectors using only
   vectorized operations (no loops).
4. **(pandas, 25 pts)** Using the provided dataset `assignment01_data.csv`:
   a. Report the number of missing values per column and handle them with a justified strategy.
   b. Compute the mean of a numeric column grouped by a categorical column.
   c. Produce one plot showing a relationship between two variables, with labeled axes/title.
5. **(pandas + reflection, 25 pts)** Merge `assignment01_data.csv` with the provided
   `assignment01_lookup.csv` on a shared key; then write a 4–6 sentence summary of the most
   interesting pattern you found in the merged data, citing specific numbers.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

# Assignment 3 — SVMs & Model Evaluation (Week 10)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 10 | **Due:** Start of Week 12

## Instructions
Submit `assignment03.ipynb` with working code and written answers for all questions, using the
provided dataset `assignment03_data.csv` (a classification dataset that is not linearly
separable).

## Questions
1. **(Kernels, 20 pts)** Fit `SVC` (inside a scaled pipeline) with linear, polynomial
   (`degree=3`), and RBF kernels; report test accuracy for each and state which kernel performs
   best on this dataset.
2. **(Hyperparameter search, 25 pts)** Use `GridSearchCV` to jointly tune `C` and `gamma` for the
   RBF-kernel SVM over a grid of your choosing (at least 4 values each); report the best
   parameters and best cross-validated score.
3. **(Nested CV, 15 pts)** Using the grid from Question 2, run a small nested cross-validation
   (e.g., 3-fold inner, 5-fold outer) and report the nested CV score; compare it to the
   `best_score_` from Question 2 and explain any difference.
4. **(Learning curve, 20 pts)** Plot a learning curve for the best model from Question 2; state
   whether it shows signs of high bias, high variance, or neither, and propose one concrete next
   step based on the diagnosis.
5. **(Written reflection, 20 pts)** In 150–250 words, explain why a single train/test split would
   have been insufficient for choosing `C` and `gamma` reliably on this dataset, referencing your
   own results from Questions 2–3.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

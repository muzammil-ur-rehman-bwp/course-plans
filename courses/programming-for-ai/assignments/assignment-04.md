# Assignment 4 — Model Tuning & Evaluation (Week 13)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 13 | **Due:** Start of Week 15

## Instructions
Submit `assignment04.ipynb` with working code and written answers for all questions.

## Questions
1. **(Cross-validation, 20 pts)** For a model and dataset of your choice (from a prior lab or
   assignment), run 5-fold and 10-fold cross-validation; compare the mean/std of scores between
   the two settings and comment on the tradeoff of fold count.
2. **(Hyperparameter tuning, 25 pts)** Define a hyperparameter grid for your chosen model and
   run `GridSearchCV`. Report the best parameters, best CV score, and test-set score using the
   best model.
3. **(Learning curves, 25 pts)** Plot training vs. validation error against training-set size
   for your tuned model. Diagnose underfitting, overfitting, or good fit, citing specific
   features of the plot (gap size, trend as data increases).
4. **(Regularization, 15 pts)** If using a linear model, compare unregularized vs. L2-regularized
   (`Ridge`/`LogisticRegression(penalty="l2")`) versions; report how regularization strength
   (`alpha`/`C`) affects the learning curve.
5. **(Reflection, 15 pts)** Write 4–6 sentences on what changed between your model's initial
   (untuned) and final (tuned) performance, and what you would try next with more time/data.

## Submission
Upload `assignment04.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

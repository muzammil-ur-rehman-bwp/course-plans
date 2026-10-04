# Week 8 Summary — Ensemble Methods II — Boosting; Midterm Review

**Key takeaways:**
- Boosting trains weak learners sequentially, each correcting the previous ensemble's errors,
  primarily reducing bias (vs. bagging's primarily variance-reducing effect).
- AdaBoost reweights misclassified examples between rounds; Gradient Boosting fits each new
  learner to the residual/gradient of the loss.
- XGBoost and LightGBM are real, widely-used optimized gradient boosting libraries, mentioned for
  awareness rather than required use.
- Too many boosting rounds or too high a learning rate can cause boosting to overfit, unlike
  Random Forests, which are robust to adding more trees.

**You should now be able to:** fit and tune AdaBoost and Gradient Boosting; explain the
structural difference between bagging and boosting; recall and apply the core techniques from
Weeks 1–8 for the midterm.

**Reminder:** Midterm Exam is next week (Week 9), covering Weeks 1–8. Assignment 2 (classification
& ensembles) was assigned this week.
**Next week:** Midterm Exam, then Support Vector Machines.

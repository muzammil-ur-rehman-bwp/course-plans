# Assignment 2 — Classification & Ensembles (Week 8)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 8 | **Due:** Start of Week 10

## Instructions
Submit `assignment02.ipynb` with working code and written answers for all questions, using the
provided dataset `assignment02_data.csv` (a binary classification dataset with some class
imbalance).

## Questions
1. **(Preprocessing, 10 pts)** Load the dataset, split into train/test (80/20, stratified,
   `random_state=42`), and report the class distribution in both splits.
2. **(Baseline classifiers, 20 pts)** Train `LogisticRegression`, `KNeighborsClassifier`, and
   `DecisionTreeClassifier` (each with reasonable default/lightly-tuned settings); report test
   accuracy, precision, recall, and F1 for each.
3. **(Ensembles, 25 pts)** Train `RandomForestClassifier`, `AdaBoostClassifier`, and
   `GradientBoostingClassifier`; report the same metrics as Question 2, and report the Random
   Forest's top 5 features by importance.
4. **(Imbalance handling, 20 pts)** Re-fit your best-performing classifier from Questions 2–3
   with `class_weight="balanced"` (or equivalent); report whether minority-class recall improves,
   and at what cost (if any) to precision/overall accuracy.
5. **(Recommendation, 25 pts)** Recommend one classifier for deployment on this dataset,
   justified with at least three specific metric values and one explicit tradeoff (e.g.,
   interpretability vs. performance, or precision vs. recall given a stated cost scenario of your
   choosing).

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

# Assignment 3 — Classification & Evaluation (Week 11)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 11 | **Due:** Start of Week 13

## Instructions
Submit `assignment03.ipynb` with working code and written answers for all questions, using the
provided dataset `assignment03_data.csv`.

## Questions
1. **(Preprocessing, 10 pts)** Load the dataset, split into train/test (80/20, `random_state=42`),
   and apply any necessary preprocessing (e.g., scaling, encoding categorical variables).
2. **(Model training, 20 pts)** Train three classifiers: `KNeighborsClassifier`,
   `DecisionTreeClassifier`, `SVC`. Report test accuracy for each.
3. **(Metrics, 25 pts)** For each classifier, compute and report precision, recall, F1, and the
   confusion matrix. Identify the class most often confused with another, and hypothesize why.
4. **(Imbalance, 20 pts)** Report the class distribution of the dataset. If imbalanced, explain
   why accuracy alone would be a misleading metric here, using your actual computed numbers as
   evidence.
5. **(Recommendation, 25 pts)** Recommend one classifier for deployment on this dataset. Justify
   your choice using at least two specific metric values and one tradeoff you considered (e.g.,
   interpretability vs. accuracy, training time vs. performance).

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

# Week 6 Lecture Plan — Introduction to Machine Learning
## Topic: Decision Trees

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain entropy, information gain, and Gini impurity as splitting criteria. (*Understand*)
2. Apply `DecisionTreeClassifier`/`Regressor` and visualize the resulting tree. (*Apply*)
3. Analyze overfitting in decision trees and apply pruning controls to address it. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Entropy & information gain | Worked numeric example on a small dataset |
| 0:25–0:45 | Gini impurity | Comparison with entropy; CART's default criterion |
| 0:45–0:55 | Break | — |
| 0:55–1:20 | Tree construction (conceptual) | Recursive splitting (ID3/CART); stopping criteria |
| 1:20–1:45 | Overfitting in trees | Fully-grown trees memorize training data |
| 1:45–2:00 | Pruning | `max_depth`, `min_samples_leaf`, cost-complexity pruning (`ccp_alpha`) |

### Materials/Equipment
- Live-coding environment, scikit-learn; a classification dataset.

### Formative Check (in-class)
Exercise: compute entropy and information gain by hand for a simple binary split on a small
dataset.

### Link to Lab/Assessment
Lab 6: decision tree lab comparing unpruned vs. pruned trees' train/test accuracy.

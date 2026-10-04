# Lab Manual 6 — Decision Trees

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Fit, visualize, and prune a decision tree, and quantify the effect of pruning on overfitting.

## Setup
Continue with scikit-learn; create `lab06.ipynb`.

## Procedure
1. **Task A — Unpruned tree:** fit a `DecisionTreeClassifier` with no depth limit on the provided
   dataset; report train and test accuracy.
2. **Task B — Visualize:** visualize the unpruned tree with `plot_tree` (or export it, if very
   large, with `max_depth` set only for the plot) and note its depth and number of leaves.
3. **Task C — Pruning sweep:** fit trees at `max_depth` in `{2, 4, 6, 10, None}`; plot train and
   test accuracy against `max_depth` on the same axes.
4. **Task D — Cost-complexity pruning:** use `cost_complexity_pruning_path` to obtain candidate
   `ccp_alpha` values; fit trees at a few of these values and report test accuracy, selecting the
   best.

## Expected Output
A notebook with Tasks A–D, including the tree visualization and the accuracy-vs-depth plot.

## Submission
Submit `lab06.ipynb` by the end of the lab session.

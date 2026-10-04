# Lab Manual 12 — Machine Learning Survey: A Tiny Decision Tree

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement a tiny ID3-style decision tree (entropy/information gain) on a toy dataset in plain
Python.

## Setup
Create `lab12.ipynb`; use the `entropy`/`information_gain` functions and "Play Tennis?" dataset
from the lecture content as a starting point.

## Procedure
1. **Task A — Entropy & information gain:** implement/paste `entropy(labels)` and
   `information_gain(examples, attribute, label_key)`; compute information gain for all
   attributes in the provided dataset and identify the best root split.
2. **Task B — Recursive tree builder:** implement `build_tree(examples, attributes, label_key)`
   that recursively splits on the best remaining attribute, stopping when all examples share a
   label or no attributes remain; represent the tree as nested dicts.
3. **Task C — Prediction:** implement `predict(tree, example)` that walks the tree to a leaf and
   returns its label; test on all training examples and report training accuracy.
4. **Task D — New example:** classify one new, unseen example (provided by the instructor or
   constructed by you) and trace, in a markdown cell, which path through the tree it follows.

## Expected Output
A notebook with Tasks A–D; the tree must achieve 100% training accuracy on the small provided
dataset (expected, since it is not held-out validation).

## Submission
Submit `lab12.ipynb` by the end of the lab session.

# Week 1 Lecture Plan — Introduction to Machine Learning
## Topic: Introduction to ML

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the history and motivation for machine learning and define supervised, unsupervised, and
   reinforcement learning. (*Remember*)
2. Explain the standard ML workflow and why a held-out test set is necessary. (*Understand*)
3. Apply `train_test_split` to partition a dataset into training, validation, and test sets.
   (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Why machine learning; brief history | Mini-lecture: rule-based systems vs. learning from data |
| 0:20–0:50 | The ML taxonomy | Supervised / unsupervised / reinforcement learning, with examples of each |
| 0:50–1:00 | Break | — |
| 1:00–1:30 | The ML workflow | Data → features → train → evaluate → iterate; where each week of the course fits |
| 1:30–1:50 | Bias-variance tradeoff (preview) | Intuition only: underfitting vs. overfitting, revisited quantitatively in Week 10 |
| 1:50–2:00 | Train/validation/test splits | Why test data must stay untouched until final evaluation |

### Materials/Equipment
- Live-coding environment, scikit-learn, a small tabular dataset (e.g., the Iris or a housing
  dataset) for demonstration only (no modeling yet).

### Formative Check (in-class)
Exercise: for five short scenario descriptions, classify each as supervised, unsupervised, or
reinforcement learning, and justify in one sentence.

### Link to Lab/Assessment
Lab 1: Python/NumPy/pandas/scikit-learn environment setup, dataset exploration, and a correct
train/validation/test split.

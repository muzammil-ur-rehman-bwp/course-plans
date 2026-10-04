# Week 12 Lecture Plan — Introduction to AI
## Topic: Introduction to Machine Learning (Survey)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why learning matters for AI and distinguish supervised from unsupervised learning. (*Understand*)
2. Apply entropy and information gain to build a small decision tree by hand. (*Apply*)
3. Evaluate, at a survey level, where classical ML fits relative to the search/logic/planning techniques covered earlier in the course. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Why learning? | An agent that improves performance from experience; limits of hand-coded rules |
| 0:20–0:45 | Supervised vs. unsupervised | Labeled vs. unlabeled data; regression/classification vs. clustering, with examples |
| 0:45–1:00 | Break | — |
| 1:00–1:30 | Decision trees | Entropy, information gain; building a small tree by hand on a toy dataset (e.g., "play tennis?") |
| 1:30–2:00 | ML in context | How this survey connects to search (tree-building as a search over splits) and logic (a tree as a set of rules); honest scope note on what this course does *not* cover (large-scale training, deep pipelines) |

### Materials/Equipment
- Slides: entropy/information-gain formulas, worked decision-tree example
- Toy dataset handout (small table of examples with discrete attributes)

### Formative Check (in-class)
Exercise: compute the information gain of two candidate attributes on the toy dataset by hand
and decide which one the tree should split on first.

### Link to Lab/Assessment
Lab 12: Implement a tiny ID3-style decision tree (entropy/information gain) on a toy dataset.

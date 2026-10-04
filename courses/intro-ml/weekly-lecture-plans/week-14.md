# Week 14 Lecture Plan — Introduction to Machine Learning
## Topic: Recommender Systems

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain content-based and collaborative filtering approaches to recommendation.
   (*Understand*)
2. Apply cosine similarity to build a simple content-based and a user/item-based collaborative
   recommender. (*Apply*)
3. Analyze the tradeoffs between content-based and collaborative approaches for a given scenario.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Content-based filtering | Item feature vectors; cosine similarity |
| 0:25–0:50 | The user-item ratings matrix | Sparsity; implicit vs. explicit feedback |
| 0:50–1:00 | Break | — |
| 1:00–1:25 | User-based collaborative filtering | Similar users, weighted rating prediction |
| 1:25–1:50 | Item-based collaborative filtering | Similar items; why it's often preferred in practice |
| 1:50–2:00 | A simple worked example | Combining content and collaborative signals |

### Materials/Equipment
- Live-coding environment, scikit-learn, pandas; a small item-feature dataset and a small
  user-item ratings matrix.

### Formative Check (in-class)
Exercise: given two users' rating vectors over the same items, compute their cosine similarity
by hand and predict a rating for a held-out item.

### Link to Lab/Assessment
Lab 14: recommender systems lab building content-based and collaborative recommenders.
**Assignment 4 assigned this week.**

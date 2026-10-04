# Week 15 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Evaluating and Debugging Neural Networks

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Analyze train/validation/test loss and accuracy curves to diagnose a network's behavior.
   (*Analyze*)
2. Evaluate whether a given training run is underfitting, overfitting, or broken, and propose a
   fix. (*Evaluate*)
3. Apply systematic hyperparameter tuning to improve a network's validation performance. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Pulling together every architecture (MLP/CNN/RNN) trained so far under one evaluation lens |
| 0:15–0:45 | Reading loss/accuracy curves | Healthy convergence, underfitting, overfitting, broken training — four reference patterns |
| 0:45–1:10 | Diagnosing a broken training loop | Common causes: bad labels, no normalization, learning rate issues, a bug in the loss |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Hyperparameter tuning basics | Learning rate, batch size, width/depth, regularization strength; systematic (grid) search |
| 1:45–2:00 | Capstone work time / Q&A | Instructor circulates for project troubleshooting |

### Materials/Equipment
- Live-coding environment, PyTorch or Keras, Matplotlib
- Handout: 4 pre-generated loss-curve plots, each illustrating one failure/success pattern

### Formative Check (in-class)
Given 4 unlabeled loss-curve plots (healthy, underfitting, overfitting, broken), each student
matches each plot to its correct diagnosis and states the most likely fix.

### Link to Lab/Assessment
Lab 15: Given several pre-generated "broken" training runs, diagnose the fault from the loss
curve alone; run a small hyperparameter sweep.

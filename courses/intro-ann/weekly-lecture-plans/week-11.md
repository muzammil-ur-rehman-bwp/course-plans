# Week 11 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Regularization

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain overfitting in neural networks as excess capacity fitting noise rather than signal.
   (*Understand*)
2. Apply L1/L2 weight regularization, dropout, and early stopping, including their effect on the
   backpropagation gradient. (*Apply*)
3. Analyze training/validation curves to select an appropriate regularization strategy and
   strength. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap & motivation | A network with enough capacity can memorize training data — why that is bad |
| 0:15–0:45 | L1/L2 regularization | Added loss penalty; effect on the gradient (weight decay) |
| 0:45–0:55 | Break | — |
| 0:55–1:25 | Dropout | Random unit deactivation at training time; inverted-dropout scaling at test time |
| 1:25–1:45 | Early stopping | Using a validation set to halt training before overfitting sets in |
| 1:45–2:00 | Capstone proposal briefing | Proposal requirements and example topics (due this week) |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib
- Plot: training/validation loss with and without regularization, same network

### Formative Check (in-class)
Given a train/validation loss curve showing a widening gap, each student proposes one specific
regularization technique and justifies it.

### Link to Lab/Assessment
Lab 11: Add L2 regularization and dropout to the from-scratch network; compare training/
validation curves with and without regularization.

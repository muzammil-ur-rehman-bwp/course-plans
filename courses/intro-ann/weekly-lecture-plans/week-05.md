# Week 5 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Loss Functions

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Apply Mean Squared Error, binary cross-entropy, and categorical cross-entropy to compute a
   loss value from predictions and labels. (*Apply*)
2. Analyze why the loss function must be paired with a matching output activation. (*Analyze*)
3. Analyze why cross-entropy avoids the flat-gradient problem MSE has when paired with a
   saturating output activation. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Forward pass produces $\hat y$; how do we quantify "how wrong"? |
| 0:15–0:35 | Mean Squared Error | Formula, use for regression, gradient shape preview |
| 0:35–0:45 | Break | — |
| 0:45–1:15 | Binary cross-entropy | Formula, pairing with sigmoid, information-theoretic motivation |
| 1:15–1:40 | Categorical cross-entropy | Formula, pairing with softmax, one-hot labels |
| 1:40–2:00 | Why MSE + sigmoid is a poor pairing | Derivative comparison showing MSE's vanishing gradient at saturation vs. cross-entropy's clean gradient |

### Materials/Equipment
- Live-coding environment, NumPy
- Plot: loss vs. prediction for MSE vs. cross-entropy at a fixed wrong label

### Formative Check (in-class)
Given three stated tasks (predicting house price; binary spam/not-spam; 10-class digit
classification), each student names the correct loss/output-activation pairing and justifies it.

### Link to Lab/Assessment
Lab 5: Implement MSE, binary cross-entropy, and categorical cross-entropy; compare gradients for
MSE vs. cross-entropy at saturation.

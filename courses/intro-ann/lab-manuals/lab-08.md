# Lab Manual 8 — A Complete From-Scratch Network, Trained End-to-End

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement and train the `NeuralNetwork` class from lecture on XOR and a second, slightly larger
toy dataset.

## Setup
Create `lab08.ipynb`.

## Procedure
1. **Task A — Class implementation:** implement `NeuralNetwork` (`forward`, `backward`,
   `update`) and `bce_loss` exactly as shown in lecture.
2. **Task B — Train on XOR:** train for 5000 epochs; plot the loss curve; print final predictions
   and confirm all four are correctly classified (rounding $\hat y$ to the nearest 0/1).
3. **Task C — Train on a larger toy set:** generate a synthetic 2D dataset with two interleaving
   classes (e.g., two concentric circles, 200 points total, via simple NumPy trigonometry plus
   noise — no scikit-learn required); train the same network (increase hidden units if needed)
   and plot the loss curve and a 2D scatter of predictions colored by predicted class.
4. **Task D — Gradient check reuse:** reuse your Week 7 finite-difference gradient-checking code
   to verify `NeuralNetwork.backward` on a small batch (2–3 examples) from Task C's dataset before
   trusting the full training run.

## Expected Output
A notebook with Tasks A–D, including both loss curves and the Task C scatter plot.

## Submission
Submit `lab08.ipynb` by the end of the lab session.

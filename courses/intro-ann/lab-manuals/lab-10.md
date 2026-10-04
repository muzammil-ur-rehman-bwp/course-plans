# Lab Manual 10 — Optimizers From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement momentum, RMSProp, and Adam from scratch and compare their convergence behavior.

## Setup
Create `lab10.ipynb`, reusing the Week 8 `NeuralNetwork` class.

## Procedure
1. **Task A — Implementations:** implement `momentum_step`, `rmsprop_step`, and `adam_step` as
   shown in lecture, each as a standalone function operating on a single parameter array.
2. **Task B — Integrate into training:** modify `NeuralNetwork.update` (or add an alternative
   update method) to support plain SGD, momentum, RMSProp, or Adam, selected by a parameter.
3. **Task C — Convergence comparison:** train the Week 9 Task C (interleaving-classes) dataset
   with each of the four optimizers, same number of epochs, tuning each optimizer's learning rate
   separately if needed for a fair comparison; plot all four loss curves on one figure.
4. **Task D — Bias correction ablation:** implement a version of Adam with bias correction
   disabled (use raw $m_t, v_t$ directly); compare its loss curve for the first 20 iterations
   against full Adam's, and explain the difference in a markdown cell.

## Expected Output
A notebook with Tasks A–D, including the four-optimizer comparison plot and the bias-correction
ablation plot.

## Submission
Submit `lab10.ipynb` by the end of the lab session.

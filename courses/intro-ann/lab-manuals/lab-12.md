# Lab Manual 12 — Training an MLP on MNIST with a Framework

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Build, train, and evaluate an MLP on MNIST (or Fashion-MNIST) using PyTorch (or Keras).

## Setup
Install PyTorch (`pip install torch torchvision`) or TensorFlow/Keras per instructor's chosen
framework. Create `lab12.ipynb`.

## Procedure
1. **Task A — Data loading:** load MNIST/Fashion-MNIST with the framework's built-in dataset
   utility; create train/validation/test splits (hold out a validation set from the training
   data, e.g., the last 5,000 training examples); print the shapes of each split.
2. **Task B — Model definition:** define an `MLP` class (or `Sequential` model) with at least one
   hidden layer, as shown in lecture.
3. **Task C — Training loop:** write the training loop (or use the framework's built-in `.fit`
   if using Keras), training for at least 5 epochs; record and plot training and validation loss
   and accuracy per epoch.
4. **Task D — Evaluation:** evaluate the trained model on the held-out test set; report test
   accuracy, and show 10 example test images alongside their predicted and true labels (including
   at least one misclassified example, if any exist).

## Expected Output
A notebook with Tasks A–D, including the training curve plots and the example-predictions figure.

## Submission
Submit `lab12.ipynb` by the end of the lab session.

# Assignment 2 — Backpropagation From Scratch (Weeks 5–8)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 8 | **Due:** Start of Week 10

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (code + brief explanation) for each question.

## Questions
1. **(Loss/activation pairing, 15 pts)** For a 5-class image classification task, state the
   correct output activation and loss function. Then show, with the general $\delta^{(L)} =
   a^{(L)} - y$ result from Week 7, why this pairing gives a clean output-layer gradient; derive
   the same result for a 3-class numeric example of your choosing.
2. **(Hand derivation, 25 pts)** For a 2-input, 3-hidden-unit (ReLU), 1-output (sigmoid) network
   with weights and a single example of your choosing, derive by hand every gradient
   ($\partial L/\partial W^{(1)}, \partial L/\partial b^{(1)}, \partial L/\partial W^{(2)},
   \partial L/\partial b^{(2)}$), showing all intermediate values ($z^{(1)}, a^{(1)}, z^{(2)},
   \hat y, \delta^{(2)}, \delta^{(1)}$). Note: ReLU's derivative is a step function, not
   sigmoid's $a(1-a)$ — use the correct one.
3. **(From-scratch implementation, 35 pts)** Implement a `NeuralNetwork` class supporting an
   arbitrary number of hidden layers (not just one) with ReLU hidden activations and a sigmoid
   output, trained with binary cross-entropy. Train it on a provided dataset
   `assignment02_data.csv` (2 features, binary label, not linearly separable); plot the loss
   curve and report final training accuracy.
4. **(Gradient check, 15 pts)** Implement a finite-difference gradient check for your Question 3
   network and confirm every parameter's analytic gradient matches the numerical approximation to
   at least 4 decimal places on a small batch.
5. **(Reflection, 10 pts)** In 4–6 sentences, describe one bug you encountered while implementing
   Question 3 (or would expect to, if none occurred), how you would detect it, and how you would
   fix it.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

# Assignment 4 — Framework, CNN, and RNN (Weeks 12–14)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 14 | **Due:** Start of Week 16

## Instructions
Submit a single Jupyter notebook `assignment04.ipynb` answering all questions below. Show your
work (code + brief explanation) for each question.

## Questions
1. **(Autograd, 15 pts)** Using PyTorch (or Keras), define a small computational graph by hand
   (not a full `nn.Module`) with at least 3 chained tensor operations and `requires_grad=True`
   inputs; call `.backward()` and compare the resulting `.grad` values against a hand-derived
   gradient for the same expression.
2. **(MLP on real data, 20 pts)** Train an MLP (framework-based) on a provided tabular or image
   dataset `assignment04_data`; report training/validation curves and test accuracy.
3. **(CNN, 25 pts)** Build and train a small CNN on the same (or a provided image) dataset;
   report its test accuracy and parameter count alongside Question 2's MLP; discuss the trade-off.
4. **(RNN/LSTM, 25 pts)** Implement an RNN cell's forward pass by hand in NumPy for a 4-step
   sequence of your choosing (if not reusing Lab 14 code); then build and train both an `nn.RNN`
   and an `nn.LSTM` model on a provided sequence dataset `assignment04_sequences`; compare their
   final performance and discuss, citing the vanishing-gradient explanation from lecture, why one
   might outperform the other.
5. **(Reflection, 15 pts)** In 5–7 sentences, compare the MLP, CNN, and RNN/LSTM architectures
   covered this semester: what structural assumption does each make about its input data, and how
   does that assumption show up in the architecture's design (weight sharing in space vs. time,
   or none at all for the plain MLP)?

## Submission
Upload `assignment04.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

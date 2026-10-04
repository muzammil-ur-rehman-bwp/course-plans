# Week 15 Summary — Backpropagation & Training with PyTorch/Keras

**Key takeaways:**
- Backpropagation applies the chain rule backward through the network to compute gradients of
  the loss with respect to every weight.
- SGD, SGD+Momentum, and Adam are optimizers that differ in how they use gradient information to
  update weights; Adam is the common practical default.
- Deep learning frameworks (PyTorch/Keras) automate both the forward pass and backpropagation
  (`loss.backward()`), but you now know what that call is actually computing.
- Loss curves (train vs. validation) diagnose healthy training vs. overfitting vs. a
  training-setup bug, directly extending Week 13's overfitting concepts.

**You should now be able to:** build and train a small feedforward network in PyTorch/Keras;
interpret training/validation loss curves.

**Reminder:** Assignment 4 due at the start of this week.
**Next week:** capstone presentations, course review, and AI ethics.

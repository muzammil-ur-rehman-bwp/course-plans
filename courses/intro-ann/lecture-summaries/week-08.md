# Week 8 Summary — Backpropagation From Scratch in NumPy; Midterm Review

**Key takeaways:**
- Week 7's hand-derived formulas translate almost line-for-line into a `forward`/`backward`/
  `update` NumPy class; the only addition is averaging gradients across a mini-batch.
- A from-scratch network with enough hidden units and a correct training loop reliably solves
  XOR — the concrete, trained resolution of the limitation first identified in Week 2.
- A loss curve that fails to decrease is diagnostic: it almost always points to a transpose error,
  a bad learning rate, or a shape/label mismatch, each traceable with Week 7's gradient-checking
  technique.
- Weeks 1–8 together form one connected argument: perceptron limitation → MLP's representational
  fix → the loss/activation pairing that makes gradients well-behaved → gradient descent →
  backpropagation as the mechanism that actually finds good weights.

**You should now be able to:** implement and train a complete feedforward network from scratch in
NumPy; diagnose a non-decreasing loss curve; synthesize Weeks 1–8 for the midterm.

**Next week:** the midterm exam, followed by weight initialization and the vanishing/exploding
gradient problem in deeper networks.

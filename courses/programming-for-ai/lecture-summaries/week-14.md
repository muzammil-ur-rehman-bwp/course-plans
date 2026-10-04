# Week 14 Summary — Neural Networks I

**Key takeaways:**
- A perceptron computes a weighted sum + bias, then applies an activation function; a single
  perceptron can only represent linearly separable functions (cannot learn XOR).
- Activation functions (sigmoid, ReLU, softmax) introduce non-linearity, which is what allows
  multi-layer networks to represent complex, non-linear functions.
- Forward propagation chains `W @ x + b` through layers, exactly extending the Week 3 NumPy
  linear algebra skills.
- The loss function (MSE, binary/categorical cross-entropy) must match the task and output
  activation.

**You should now be able to:** compute a forward pass by hand and in NumPy; choose an
appropriate activation/loss pairing for a given task.

**Next week:** backpropagation and training a network end-to-end with PyTorch/Keras.

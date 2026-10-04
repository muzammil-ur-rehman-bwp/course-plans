# Lab Notes 13 — Neural Networks Survey: A Perceptron from Scratch

**Concept recap:** the perceptron learning rule nudges weights toward correctly classifying a
misclassified example; it provably converges if and only if the training data is linearly
separable, which is why it succeeds on AND/OR but can never succeed on XOR.

**Common pitfalls:**
- Using a learning rate so large that weights oscillate instead of converging — if AND/OR don't
  converge within a reasonable number of epochs, try a smaller learning rate before assuming a
  logic bug.
- Expecting the XOR training loop to eventually "break through" with more epochs — it will not,
  by the linear-separability argument; running more epochs on XOR is a useful empirical
  demonstration, not a bug to fix.
- Forgetting to reset weights/bias to 0 (or small random values) before training a *new*
  perceptron on a different dataset in the same notebook — reusing a trained perceptron object
  silently carries over old weights.

**Debugging tip:** log total error per epoch for every training run; a converging perceptron's
error should trend to 0 and stay there, while a non-separable dataset's error will plateau above
0 or oscillate indefinitely — plotting this (even as plain printed numbers) makes the
linear-separability argument visible, not just theoretical.

**Instructor tip:** have students sketch the AND/OR/XOR truth tables as points on a 2D plane
*before* running any code and try to draw a single separating line by hand — failing to do so
for XOR is a more convincing demonstration than any code output.

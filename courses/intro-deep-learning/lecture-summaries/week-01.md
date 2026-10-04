# Week 1 Summary — The Deep Learning Landscape and the PyTorch Framework Tour

**Key takeaways:**
- This course builds directly on the prerequisite course's perceptron → MLP → backpropagation →
  optimizer pipeline; none of that is re-derived here.
- Depth matters because it enables hierarchical, learned feature representations instead of
  hand-engineered features.
- PyTorch tensors with `requires_grad=True` and `.backward()` are autograd's mechanization of the
  same backpropagation already understood from the prerequisite course.
- The canonical PyTorch training loop (`zero_grad → forward → loss → backward → step`) is the
  tool used for every remaining week of this course.

**You should now be able to:** state the prerequisite pipeline this course assumes; explain why
depth matters in deep learning; create tensors and use autograd; write an `nn.Module` and a
training loop in PyTorch.

**Next week:** deep networks in practice — initialization, batch normalization, dropout, and
gradient flow, revisited at implementation depth.

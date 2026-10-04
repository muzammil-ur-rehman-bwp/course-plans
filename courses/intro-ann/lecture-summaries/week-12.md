# Week 12 Summary — Introduction to a Deep Learning Framework

**Key takeaways:**
- Framework autograd is the same chain-rule computation derived by hand in Week 7 and coded by
  hand in Week 8, generalized to automatically track and differentiate arbitrary computational
  graphs.
- A model's `forward` method is the Week 4 forward pass; `loss.backward()` is the Week 7/8
  backward pass; `optimizer.step()` is the Week 10 parameter update, for whichever optimizer is
  configured.
- `optimizer.zero_grad()` is required because gradients accumulate by default; omitting it
  corrupts training by adding each batch's gradient onto the last.
- `model.eval()` / `torch.no_grad()` at evaluation time is directly analogous to the Week 11
  `training=False` flag for dropout.

**You should now be able to:** build, train, and evaluate an MLP on a real dataset (MNIST/
Fashion-MNIST) using a deep learning framework; relate every line of a framework training loop
back to the from-scratch concepts built in Weeks 4–10.

**Next week:** convolutional neural networks — a new architecture, still trained by the exact
same autograd-based loop, specialized for image data.

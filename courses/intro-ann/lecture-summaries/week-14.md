# Week 14 Summary — Recurrent Neural Networks (Basics)

**Key takeaways:**
- RNNs maintain a hidden state updated by the same shared weights at every time step:
  $h_t = \tanh(W_{xh}x_t + W_{hh}h_{t-1} + b_h)$, giving the network a notion of memory that MLPs
  and CNNs lack.
- Backpropagation through time (BPTT) is the Week 7 chain rule applied along the time axis;
  sequence length plays the role depth played in Week 9's vanishing/exploding gradient discussion.
- Plain RNNs struggle with long-range dependencies because of this same vanishing-gradient
  mechanism, now driven by sequence length.
- LSTMs address this with a mostly-additive cell-state update, gated by learned forget/input/
  output gates, letting gradients flow across many time steps with far less shrinkage.
- This week is deliberately brief; full gate-equation derivations and GRU/attention variants
  belong to a dedicated Deep Learning course.

**You should now be able to:** compute an RNN cell's hidden state across a short sequence by
hand; explain why plain RNNs suffer vanishing gradients over long sequences; describe, at a
conceptual level, how LSTMs mitigate this.

**Next week:** evaluating and debugging neural networks — reading training curves, diagnosing
failures, and tuning hyperparameters, across every architecture covered this semester.

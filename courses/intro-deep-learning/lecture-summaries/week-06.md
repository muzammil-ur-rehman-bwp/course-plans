# Week 6 Summary — Sequence Models I: RNN Limitations, LSTM, GRU

**Key takeaways:**
- Vanilla RNNs struggle with long sequences because backpropagation through time repeatedly
  multiplies by the same recurrent weight matrix, causing vanishing or exploding gradients.
- The LSTM cell uses forget, input, and output gates plus a cell state to control what information
  is kept, added, and exposed at each time step, with a largely additive cell-state update that
  preserves gradient flow.
- The GRU simplifies this to two gates (update, reset) and no separate cell state, with fewer
  parameters than an LSTM.
- `nn.LSTM`/`nn.GRU` implement these equations directly; the student's job is to understand what
  they compute, not re-derive backpropagation through them.

**You should now be able to:** write out the LSTM and GRU gate equations; explain why vanilla RNNs
struggle with long sequences; use `nn.LSTM`/`nn.GRU` and trace tensor shapes through a sequence.

**Next week:** sequence models II — sequence-to-sequence architectures and applications.

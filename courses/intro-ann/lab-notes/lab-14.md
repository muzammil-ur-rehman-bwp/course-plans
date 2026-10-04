# Lab Notes 14 — An RNN Cell From Scratch and a Minimal Sequence Model

**Concept recap:** an RNN cell reuses the same weights at every time step; `nn.RNN`/`nn.LSTM`
expect input shaped `(batch, seq_len, input_size)` (with `batch_first=True`), and return both the
per-step outputs and the final hidden state.

**Common pitfalls:**
- Mixing up $W_{xh}$ (applied to the input) and $W_{hh}$ (applied to the previous hidden state)
  in Task A's by-hand implementation — their shapes often coincide for small examples, masking a
  swapped-matrix bug; checking against the hand-computed values from lecture catches this.
- Forgetting to reset/reinitialize the hidden state ($h_0$) between independent sequences in a
  batch — carrying over the previous sequence's final hidden state into the next, unrelated
  sequence silently corrupts training.
- Building Task B's dataset with sequences too short to make the RNN-vs-LSTM distinction
  meaningful — the vanishing-gradient advantage of LSTMs over plain RNNs only shows up once
  dependencies span a reasonably long sequence; very short sequences may show little difference.
- Treating `out[:, -1, :]` (the last time step) as the only usable output when the task actually
  needs a prediction at every time step — check what Task B's task requires before deciding which
  time step(s) of `out` to use.

**Debugging tip:** for Task A, print the hidden state after every time step, not just the final
one — a sign or transpose error is much easier to spot one step at a time than only at the end of
a 3-step sequence.

**Instructor tip:** Task D's longer-sequence generalization test is optional but valuable — even
a small, informal demonstration that LSTMs degrade less than plain RNNs on longer sequences than
seen during training makes the vanishing-gradient discussion concrete rather than purely
theoretical.

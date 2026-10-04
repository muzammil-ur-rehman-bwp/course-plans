# Lab Notes 7 — A Sequence-to-Sequence Model in PyTorch

**Concept recap:** the encoder's final hidden/cell state initializes the decoder; teacher forcing
uses the true previous target during training, while generation at inference feeds back the
model's own previous prediction.

**Common pitfalls:**
- Forgetting to pass the encoder's final `(h_n, c_n)` into the decoder's initial state, instead
  leaving the decoder's state at its default (zero) initialization — this silently discards all
  information the encoder computed.
- Off-by-one errors in teacher forcing's input/target slicing (`y[:, :-1, :]` as input vs.
  `y[:, 1:, :]` as target) — verify shapes and a couple of example rows explicitly before training.
- In Task C's autoregressive generation, accidentally continuing to use teacher forcing (feeding
  the true target) instead of the model's own previous output — this will look deceptively
  good in a quick check and only reveal itself as a bug when evaluated honestly on new data.
- Not detaching or re-initializing hidden state between independent generated sequences in a
  batch loop, leaking state across unrelated examples.

**Debugging tip:** if Task D's generated outputs look reasonable for the first step but degrade
quickly afterward, check whether that is genuine exposure bias (expected, and worth discussing)
or a bug in how `decoder_input` is updated each generation step (feeding the wrong tensor, or the
right tensor with the wrong shape).

**Instructor tip:** have students generate at least one example by feeding the model garbage input
and observe the output — this makes the autoregressive feedback loop's behavior (and exposure
bias) visually obvious in a way the loss curve alone does not.

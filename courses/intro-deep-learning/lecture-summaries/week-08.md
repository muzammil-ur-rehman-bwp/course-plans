# Week 8 Summary — Attention Mechanisms; Midterm Review

**Key takeaways:**
- Basic seq2seq compresses an entire input sequence into one fixed-length context vector, which
  becomes an information bottleneck for long sequences.
- Attention replaces that single vector with a learned weighted combination of all encoder hidden
  states, recomputed for each decoding step.
- Attention scores (dot-product or additive/Bahdanau) are converted to weights via softmax, and
  the weighted sum of encoder states gives the context vector used at that decoding step.
- Weeks 1–8 form one arc: deep-net practice → CNNs → sequence models → attention, all built on the
  prerequisite course's perceptron/MLP/backprop/optimizer foundation.

**You should now be able to:** explain the seq2seq bottleneck problem; compute attention scores,
softmax weights, and a context vector by hand and in PyTorch; synthesize Weeks 1–8 for the midterm.

**Next week:** midterm exam, followed by an introduction to the Transformer architecture.

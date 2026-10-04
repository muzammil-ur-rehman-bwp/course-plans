# Week 9 Summary — Midterm Exam + Introduction to Transformers

**Key takeaways:**
- Self-attention lets every position in a sequence attend to every other position in the same
  sequence, computed via scaled dot-product attention.
- Multi-head attention runs several attention computations in parallel on different learned
  projections of the input and combines the results, letting the model capture different kinds of
  relationships simultaneously.
- Because self-attention itself has no notion of order, positional encoding injects position
  information into the input representations.
- The Transformer's encoder-decoder architecture is built from stacked self-attention,
  multi-head attention, and feed-forward sublayers — this week is a survey of that architecture,
  not a from-scratch implementation.

**You should now be able to:** compute scaled dot-product self-attention for a small example;
explain multi-head attention and positional encoding conceptually; describe the overall
Transformer encoder-decoder architecture at a high level.

**Next week:** autoencoders — the encoder-decoder architecture applied to unsupervised
reconstruction.

# Week 7 Summary — Sequence Models II: Sequence-to-Sequence Architectures

**Key takeaways:**
- A sequence-to-sequence model maps a variable-length input sequence to a variable-length output
  sequence via an encoder (compresses the input) and a decoder (generates the output).
- Text generation and time-series forecasting are both naturally framed as seq2seq problems.
- Teacher forcing feeds the true previous target token during training, which speeds up and
  stabilizes training but creates a train/inference mismatch ("exposure bias") since generation at
  inference time must feed back its own previous predictions.
- The same LSTM/GRU building blocks from Week 6 compose directly into an encoder-decoder pair.

**You should now be able to:** build and train an LSTM-based encoder-decoder model in PyTorch;
explain teacher forcing and the exposure-bias trade-off it introduces.

**Next week:** attention mechanisms — overcoming the fixed-context bottleneck of basic seq2seq;
midterm review.

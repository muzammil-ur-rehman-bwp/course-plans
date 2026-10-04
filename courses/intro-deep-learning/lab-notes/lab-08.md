# Lab Notes 8 — Attention Score Computation; Midterm Review

**Concept recap:** attention scores are converted to weights via softmax (which must sum to 1
across the encoder positions, not across the batch or hidden dimension); the context vector is a
weighted sum of encoder states using those weights.

**Common pitfalls:**
- Applying `softmax` along the wrong tensor dimension — scores have shape `(batch, seq_len)`, and
  softmax must be applied along `dim=1` (the sequence dimension), not `dim=0` (the batch
  dimension).
- Using `torch.bmm` with mismatched shapes — `encoder_states` must be `(batch, seq_len, hidden)`
  and the query reshaped to `(batch, hidden, 1)` for the first `bmm` call in dot-product
  attention, as shown in lecture; a transposed dimension produces a shape that still runs but
  computes the wrong thing.
- Forgetting that adding attention changes the decoder's input size (it now also receives a
  context vector each step) — the decoder's input linear layer/LSTM input size must be updated
  accordingly, not left at Lab 7's original dimension.
- During midterm review, treating topics from different weeks as isolated facts to memorize
  rather than connecting them (e.g., not seeing that attention's softmax step is the same softmax
  from the multi-class classification output layer studied in the prerequisite course).

**Debugging tip:** print `weights.sum(dim=1)` after computing attention weights — it must be a
tensor of all 1s (up to floating-point precision); if not, the softmax dimension is wrong.

**Instructor tip:** Task D's self-assessment is meant to directly inform how students spend their
remaining midterm study time — review it before the midterm review session rather than after, so
common weak points can be addressed as a class.

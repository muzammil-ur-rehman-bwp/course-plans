# Lab Notes 6 — LSTM and GRU Gate Mechanics

**Concept recap:** `nn.LSTM`/`nn.GRU` expect input shape `(batch, seq, features)` when
`batch_first=True` (the default is `(seq, batch, features)` if `batch_first` is omitted); the
hidden/cell state shapes are `(num_layers, batch, hidden_size)` regardless of `batch_first`.

**Common pitfalls:**
- Input-shape confusion between `(batch, seq, features)` and `(seq, batch, features)` — always
  pass `batch_first=True` explicitly when constructing `nn.LSTM`/`nn.GRU`, and double-check
  tensor shapes with `.shape` immediately after loading a batch, rather than assuming the
  convention.
- Forgetting that `nn.LSTM` returns `(output, (h_n, c_n))` while `nn.GRU` returns
  `(output, h_n)` (no cell state) — code that handles both models generically must branch on this
  difference.
- In Task A/B's by-hand implementation, using the wrong concatenation order for `[h_{t-1}, x_t]`
  relative to how the provided weight matrices were constructed, producing a shape mismatch or
  (worse) a shape match with wrong values.
- Not detaching hidden/cell states between unrelated sequences when reusing a stateful loop
  structure, which can leak gradient history across examples that should be independent.

**Debugging tip:** when the by-hand LSTM/GRU implementation does not match `nn.LSTMCell`/
`nn.GRUCell`'s output, verify gate-by-gate (print `f_t`, `i_t`, `o_t` separately) rather than only
comparing the final `h_t` — this localizes the error to a specific gate equation.

**Instructor tip:** Task D's long-sequence stress test is worth discussing explicitly in the next
lecture — it is the most direct, hands-on evidence students get for *why* the gating mechanism
matters, beyond the abstract vanishing-gradient argument from lecture.

# Week 9 Summary — Midterm Exam; Speculative Decoding

**Key takeaways:**
- Speculative decoding uses a small draft model to propose $k$ tokens and verifies them with one
  parallel target-model forward pass.
- The accept/reject/resample rule ($\min(1,p/q)$ acceptance; residual resampling on rejection) is
  proven, by direct computation, to produce an output distribution exactly equal to the target
  model's — a sampling-equivalent speedup, not an approximation.
- Speedup comes from amortizing one expensive forward pass over multiple accepted tokens whenever
  the draft model agrees with the target reasonably often.
- A rejection requires rolling the KV cache back to the last accepted position before continuing.

**You should now be able to:** state and verify the speculative-decoding correctness proof;
implement the accept/reject/resample loop; explain KV-cache rollback on rejection.

**Next week:** Neural architecture search — the search-space/search-strategy/performance-
estimation decomposition, RL-based, evolutionary, and differentiable search strategies.

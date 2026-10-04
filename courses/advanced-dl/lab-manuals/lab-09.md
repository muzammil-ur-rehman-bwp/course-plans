# Lab Manual 9 — Speculative Decoding Simulation

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement a toy speculative-decoding accept/reject/resample loop, empirically verify its
correctness property, and measure its speedup behavior.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab09.ipynb`.

## Procedure
1. **Task A — Correctness verification:** implement `speculative_step` exactly as in the Week 9
   lecture content; using a hand-chosen 3-symbol toy vocabulary with given $p,q$, run 10,000
   trials and tabulate the empirical output-token frequency, confirming it matches $p$ to within
   sampling noise.
2. **Task B — Block simulation:** implement `speculative_decode_block` exactly as in lecture,
   using toy categorical "models" for draft/target at each position; run it for at least 500
   blocks with $k=4$.
3. **Task C — Agreement sweep:** construct two draft-model settings — one that agrees closely
   with the target model's distributions, one that disagrees substantially — and measure the
   average number of accepted tokens per block in each setting.
4. **Task D — KV-cache reasoning:** write a precise, 100–150 word description of exactly what
   must be rolled back in the KV cache when a draft token at position $j$ is rejected, and why
   positions before $j$ do not need to be touched.

## Expected Output
A notebook with four clearly labeled sections (A–D), including the Task A frequency table and the
Task C agreement comparison.

## Submission
Submit `lab09.ipynb` via the course submission system by the end of the lab session.

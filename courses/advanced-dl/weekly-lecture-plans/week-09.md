# Week 9 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Midterm Exam + Efficient Inference II — Speculative Decoding

**Duration:** 2 hours lecture (midterm + lecture) + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Complete the Weeks 1–8 qualifying-exam-style midterm. (*Remember–Analyze*)
2. State and verify speculative decoding's accept/reject/resample rule's exactness property.
   (*Analyze*)
3. Explain KV-cache rollback on rejection, conceptually. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Closed-book, qualifying-exam style, covers Weeks 1–8 |
| 1:00–1:15 | Motivation | Why autoregressive decoding is latency-bound; the draft-model idea |
| 1:15–1:45 | The accept/reject/resample rule | Derived and verified exact on a toy example |
| 1:45–2:00 | KV-cache rollback | What must be discarded/recomputed on a rejection |

### Materials/Equipment
- Midterm exam booklet/online exam system
- Slides: "Efficient Inference II: Speculative Decoding"
- Whiteboard for the correctness-proof derivation

### Formative Check (in-class)
For a 3-symbol toy vocabulary with given draft distribution $q$ and target distribution $p$,
compute the acceptance probability at each symbol and the residual resampling distribution,
and verify by direct summation that the resulting marginal equals $p$.

### Link to Lab/Assessment
Lab 9: implement a toy speculative-decoding simulation (draft/target toy categorical models,
accept/reject/resample loop) and empirically confirm the output distribution matches the target
model's distribution (see `lab-manuals/lab-09.md`). Assignment 2 assigned this week (Weeks 6–9).

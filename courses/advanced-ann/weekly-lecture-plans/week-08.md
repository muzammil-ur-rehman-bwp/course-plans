# Week 8 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: Scaling Laws; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the empirical power-law form relating model size, data size, compute, and loss.
   (*Understand*)
2. Evaluate theoretical attempts to explain why scaling laws hold, including their connection to
   this course's own NTK/mean-field material. (*Analyze, Evaluate*)
3. Synthesize Weeks 1–8 for the midterm exam. (*Remember, Understand, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:30 | The empirical form | Power-law loss curves in model size, data size, and compute (Kaplan et al.); fitting a scaling exponent |
| 0:30–0:55 | Compute-optimal scaling | The refinement broadly attributed to Hoffmann et al.: scale model and data size together under a fixed compute budget |
| 0:55–1:25 | Theoretical attempts | Data-manifold/intrinsic-dimension arguments; random-feature/kernel-theoretic connections back to Weeks 2–3 |
| 1:25–2:00 | Midterm review | Structured recap across Weeks 1–8; practice questions |

### Materials/Equipment
- Slides: "Scaling Laws: Empirical Form and Theoretical Attempts"
- Midterm review handout spanning Weeks 1–8

### Formative Check (in-class)
Given a log-log loss-vs-model-size plot with a measured slope, state the fitted scaling exponent
and explain, in one sentence, what doubling model size predicts for loss under that fit.

### Link to Lab/Assessment
Lab 8: fit a power-law curve to loss-vs-size data and read off the scaling exponent (see
`lab-manuals/lab-08.md`). **Midterm exam next week (Week 9), covering Weeks 1–8.**

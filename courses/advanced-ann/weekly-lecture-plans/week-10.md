# Week 10 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: Double Descent Revisited Rigorously

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the full double-descent curve across model size, sample size, and training time.
   (*Understand*)
2. Analyze the interpolation threshold as the organizing concept across all three axes.
   (*Analyze*)
3. Evaluate the theoretical explanations proposed for double descent against what they do and do
   not establish. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Graduate course's conceptual double descent (model-size axis only) |
| 0:15–0:45 | The three axes | Model-size, sample-size, and epoch-wise double descent, each organized around an interpolation threshold |
| 0:45–1:15 | Nakkiran et al.'s characterization | The rigorous, broader empirical picture |
| 1:15–1:45 | Theoretical explanations | Effective-capacity accounts tied to the interpolation threshold; connections to random-matrix-theoretic material |
| 1:45–2:00 | Synthesis | What explains the curve's *location* vs. its *magnitude* |

### Materials/Equipment
- Slides: "Double Descent: Three Axes, One Organizing Concept"
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Explain why "more training data always helps" can be locally false near a sample-size-dependent
interpolation threshold, using the same organizing concept that explains model-size double
descent.

### Link to Lab/Assessment
Lab 10: reproduce a model-size double-descent curve on a controlled dataset and identify the
interpolation threshold (see `lab-manuals/lab-10.md`).

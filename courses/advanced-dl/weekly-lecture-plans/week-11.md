# Week 11 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Scaling Laws from a Systems/Engineering Perspective

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the $C\approx 6ND$ training-FLOPs approximation and use it to compute a compute-optimal
   $(N,D)$ allocation. (*Apply*)
2. Contrast this week's compute-allocation-in-practice question with the sibling course's
   theoretical "why do power laws hold" question. (*Evaluate*)
3. Compute an optimal checkpoint interval given a failure rate and checkpoint cost. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Scoping | Explicit boundary vs. *Advanced Artificial Neural Network* Week 8's theoretical treatment |
| 0:15–0:45 | Compute-optimal allocation | The $C\approx 6ND$ approximation; growing $N,D$ together at fixed $C$ |
| 0:45–1:10 | The practical consequence | Why over-training a model past its data-optimal point wastes compute |
| 1:10–1:40 | Checkpointing and fault tolerance | Checkpoint-frequency/storage/lost-compute tradeoff; elastic restart |
| 1:40–2:00 | Synthesis | Recap table: theory question vs. systems question, side by side |

### Materials/Equipment
- Slides: "Scaling Laws: Systems/Engineering Perspective"
- Whiteboard for the compute-allocation and checkpoint-interval derivations
- Live-coding environment (Jupyter, NumPy, Matplotlib)

### Formative Check (in-class)
Given a fixed compute budget $C$ and a provided fitted scaling-exponent relationship, compute the
compute-optimal $(N,D)$ pair and compare it to a naive "double $N$, keep $D$ fixed" allocation at
the same total compute.

### Link to Lab/Assessment
Lab 11: compute-optimal allocation exercise and an optimal-checkpoint-interval calculation on a
toy training-run specification (see `lab-manuals/lab-11.md`).

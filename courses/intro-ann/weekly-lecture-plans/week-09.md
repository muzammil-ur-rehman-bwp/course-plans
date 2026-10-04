# Week 9 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Midterm Exam + Weight Initialization & Vanishing/Exploding Gradients

**Duration:** 2 hours lecture + 3 hour lab (exam occupies part of lecture time)

### Learning Objectives (Bloom's Level)
1. Demonstrate recall and application of Weeks 1–8 material under exam conditions. (*Remember, Apply*)
2. Explain why all-zero (or all-equal) weight initialization fails due to symmetry. (*Understand*)
3. Analyze, conceptually, why gradients can vanish or explode in deeper networks, and how
   initialization and activation choice both contribute. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Closed-book/open-notes per instructor policy, covers Weeks 1–8 |
| 1:00–1:10 | Break | — |
| 1:10–1:30 | Why zero initialization fails | Symmetry argument: identical hidden units receive identical gradients forever |
| 1:30–1:45 | Small random initialization | Breaking symmetry; still not sufficient for deep networks |
| 1:45–2:00 | Xavier/Glorot & He initialization; vanishing/exploding gradients | Formulas and the conceptual link to activation choice (Week 3) and network depth |

### Materials/Equipment
- Midterm exam paper/online quiz
- Slides: "Why Initialization Matters"

### Formative Check (in-class)
Post-exam discussion: given a 2-hidden-unit layer initialized with identical weights, predict
(and then verify symbolically) that both units' gradients remain identical after one backprop
step.

### Link to Lab/Assessment
Lab 9: Compare training behavior under zero, small-random, Xavier, and He initialization on the
Week 8 network, and observe activation statistics across layers in a deeper network.

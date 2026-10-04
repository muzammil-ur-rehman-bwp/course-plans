# Week 9 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Midterm Exam; Markov Decision Processes II

**Duration:** 2 hours lecture + 3 hour lab/seminar (lecture time split with the exam)

### Learning Objectives (Bloom's Level)
1. Demonstrate mastery of Weeks 1–8 material under exam conditions. (*Remember–Apply*)
2. Implement policy iteration and explain why it converges in finitely many iterations for a
   finite MDP. (*Apply, Analyze*)
3. Explain the exploration-exploitation tradeoff and implement tabular Q-learning with an
   ε-greedy policy. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Covers Weeks 1–8 |
| 1:00–1:25 | Policy iteration | Policy evaluation + policy improvement, convergence argument |
| 1:25–1:50 | Exploration-exploitation & Q-learning | ε-greedy policies; the Q-learning update rule |
| 1:50–2:00 | Synthesis | Value iteration vs. policy iteration vs. Q-learning: model-based vs. model-free |

### Materials/Equipment
- Midterm exam booklet/online exam
- Slides: "Policy Iteration & Q-Learning"
- Live-coding environment (Jupyter)

### Formative Check (in-class)
After the exam: given a small MDP and an arbitrary initial policy, perform one round of policy
evaluation and one policy-improvement step by hand.

### Link to Lab/Assessment
Lab 9: implement policy iteration and tabular Q-learning on the Week 8 grid-world
(see `lab-manuals/lab-09.md`).

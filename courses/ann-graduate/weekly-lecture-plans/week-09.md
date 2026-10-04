# Week 9 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Midterm Exam; Generalization Theory I

**Duration:** 2 hours lecture + 3 hour lab (lecture time split with the exam)

### Learning Objectives (Bloom's Level)
1. Demonstrate mastery of Weeks 1–8 material under exam conditions. (*Remember–Analyze*)
2. State the PAC learning framework and explain what it means for a hypothesis class to be
   PAC-learnable. (*Understand*)
3. Define VC dimension via shattering and compute it for a simple classical hypothesis class.
   (*Apply, Analyze*)
4. Explain why classical VC-dimension generalization bounds struggle to explain deep network
   generalization. (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Covers Weeks 1–8 |
| 1:00–1:20 | PAC learning | The framework: polynomially many samples, high-probability, low-error guarantee |
| 1:20–1:45 | VC dimension | Shattering; worked example — the VC dimension of linear threshold classifiers in the plane is 3 |
| 1:45–2:00 | The classical-bound problem | The VC bound predicts overfitting for networks with far more parameters than examples — a prediction deep learning regularly violates |

### Materials/Equipment
- Midterm exam booklet/online exam
- Slides: shattering diagram for the 2D linear-threshold example

### Formative Check (in-class)
After the exam: given 3 points in general position in the plane, students show by diagram that
every one of the 8 possible binary labelings can be realized by some linear threshold classifier
(shattering), and explain why a 4th point generically cannot always be added while preserving this.

### Link to Lab/Assessment
Lab 9: Implement a VC-dimension shattering demonstration for simple hypothesis classes (e.g.,
1D intervals, 2D linear thresholds) and empirically illustrate the gap between empirical risk and
true risk as sample size grows (see `lab-manuals/lab-09.md`).

# Week 9 Lecture Plan — Deep Learning (Graduate)
## Topic: Midterm Exam; Graph Neural Networks II

**Duration:** 2 hours lecture (midterm + lecture) + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Demonstrate mastery of Weeks 1–8 content under exam conditions. (*Remember–Analyze*)
2. Explain GraphSAGE-style neighborhood sampling and aggregation. (*Analyze*)
3. Derive and implement Graph Attention Network (GAT) attention-weighted aggregation. (*Apply,
   Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Closed-book/notes per instructor policy, covers Weeks 1–8 |
| 1:00–1:10 | Break | — |
| 1:10–1:35 | GraphSAGE | Fixed-size neighborhood sampling for scalability; mean/pooling aggregators; concatenation with self-representation |
| 1:35–2:00 | Graph Attention Networks | Learned attention coefficients over neighbors; softmax normalization; multi-head extension |

### Materials/Equipment
- Exam papers/exam platform
- Slide diagrams: GraphSAGE sampled-neighborhood illustration; GAT attention-weighted edges

### Formative Check (in-class)
Post-exam: given a node with 3 neighbors and raw attention scores, students compute the
softmax-normalized GAT attention weights by hand.

### Link to Lab/Assessment
Lab 9: Implementing GraphSAGE-style mean aggregation and GAT-style attention aggregation, and
comparing both against Week 8's GCN on the same small graph (see `lab-manuals/lab-09.md`).

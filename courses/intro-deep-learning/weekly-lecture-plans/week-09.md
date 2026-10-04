# Week 9 Lecture Plan — Introduction to Deep Learning
## Topic: Midterm Exam + Introduction to Transformers

**Duration:** 2 hours lecture + 3 hour lab (lecture time split with the exam)

### Learning Objectives (Bloom's Level)
1. Demonstrate mastery of Weeks 1–8 content on the midterm exam. (*Remember–Analyze*)
2. Explain self-attention and multi-head attention at a conceptual level. (*Understand*)
3. Describe the overall encoder-decoder Transformer architecture, including positional encoding,
   at a high level. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | **Midterm Exam** | Covers Weeks 1–8 |
| 1:00–1:10 | Break | — |
| 1:10–1:30 | Self-attention | Every position attends to every other position in the same sequence; scaled dot-product formula |
| 1:30–1:45 | Multi-head attention (conceptual) | Parallel attention computations on different learned projections, combined |
| 1:45–1:55 | Positional encoding | Why self-attention alone is order-agnostic; injecting position information |
| 1:55–2:00 | Architecture survey | Encoder stack / decoder stack diagram at a high level — a survey, not a full implementation |

### Materials/Equipment
- Midterm exam materials
- Transformer encoder-decoder architecture diagram handout

### Formative Check (in-class)
After the exam, given a 4-token sequence, state in words (not full computation) what
self-attention computes for token 2 and why positional encoding is still needed even though
attention sees every token.

### Link to Lab/Assessment
Lab 9: Implement scaled dot-product self-attention (and a simplified multi-head version) on a
small example sequence in PyTorch.

### Assessment Note
**Assignment 3** (attention and Transformers) is assigned this week.

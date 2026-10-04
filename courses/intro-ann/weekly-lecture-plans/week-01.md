# Week 1 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Biological Inspiration, History, and Course Roadmap

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the key historical milestones in neural network research. (*Remember*)
2. Explain the biological neuron analogy and the McCulloch-Pitts neuron model. (*Understand*)
3. Understand how this course's dedicated, 16-week treatment of neural networks relates to and
   extends the brief Weeks 14–15 introduction given in *Programming for Artificial Intelligence*.
   (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Course roadmap | Syllabus walkthrough; how this course differs from/extends prior AI programming course |
| 0:15–0:35 | The biological neuron | Dendrites/soma/axon/synapse; neurons "fire" based on accumulated input — loose inspiration only |
| 0:35–1:00 | History of neural networks | Timeline: McCulloch-Pitts (1943) → Hebbian learning → Rosenblatt's perceptron (1958) → Minsky-Papert critique (1969) → backpropagation (1986) → deep learning resurgence (2012) |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | The McCulloch-Pitts neuron | Binary threshold logic unit; equations; implementing AND/OR/NOT with fixed weights |
| 1:40–2:00 | Preview of the perceptron | How Week 2's perceptron generalizes the McCulloch-Pitts neuron by learning its weights |

### Materials/Equipment
- Slides: "A Brief History of Neural Networks"
- Timeline handout
- Environment setup guide (Python, NumPy, Matplotlib, Jupyter/Colab)

### Formative Check (in-class)
Given a shuffled list of historical events, order them correctly; compute the output of a
McCulloch-Pitts neuron by hand for a small set of binary inputs and fixed weights/threshold.

### Link to Lab/Assessment
Lab 1: Set up the course Python/NumPy environment; implement a McCulloch-Pitts neuron and use it
to realize AND, OR, and NOT.

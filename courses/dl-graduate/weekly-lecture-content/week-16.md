# Week 16 — Lecture Content: Capstone Research Presentations; Course Review

## 1. Presentation Format

Each capstone presentation follows a conference-talk structure: **problem** (motivating question)
→ **related work** (where the 3–5 reviewed papers sit) → **method/experiment** (what was actually
run, and why) → **results** (reported honestly, including negative/partial results) →
**limitations** (at least one, stated specifically) → **Q&A**. See
`presentations/capstone-presentation-template.md` for the student-facing slide-by-slide template
and `assignments/capstone-rubric.md` for grading criteria.

## 2. Course Map Recap

| Module | Weeks | Core Content |
|---|---|---|
| Advanced Architectures: CNNs & Transformers | 1–4 | ResNet/DenseNet/efficiency CNNs; the Transformer from scratch; BERT/GPT/ViT variants |
| Self-Supervised & Generative Models | 5–7 | InfoNCE/contrastive learning; normalizing flows; diffusion models |
| Graph Neural Networks & Deep RL | 8–11 | Message passing/GCN/GraphSAGE/GAT; DQN; REINFORCE/actor-critic |
| Scaling, Multimodal Models, Research & Capstone | 12–16 | Parallelism/mixed precision/LoRA; multimodal survey; research methods; capstone |

## 3. Connecting Back to the Sibling Graduate Courses

- *Artificial Intelligence*, Graduate's tabular MDP/RL formalism (Bellman equation, value/policy
  iteration, tabular Q-learning) was the foundation Weeks 10–11 built the deep-RL function-
  approximation extensions on top of.
- *Artificial Neural Network*, Graduate's theory — automatic differentiation as reverse-mode AD,
  initialization/normalization theory, loss-landscape geometry, generalization theory (double
  descent, NTK, Lottery Ticket Hypothesis) — underlies *why* many of this course's architectures
  and techniques train the way they do; students who want the theoretical account of, for
  example, why skip connections help optimization, or why overparameterized networks generalize,
  should pursue that course's material directly.
- *Machine Learning*, Graduate's classical statistical learning theory is the comparison point
  for understanding what is genuinely new (and what is not) about deep learning's generalization
  behavior.

## 4. Where This Leads Next

This course's architectural depth (Transformers, diffusion models, GNNs) and its deep-RL
extensions are active, fast-moving research areas; the capstone's literature-review-plus-
experiment structure is deliberately the same structure used in the courses' sibling capstones,
so that the research-methods skill (reading, critiquing, reproducing, and honestly reporting
results) transfers directly to further independent research, a thesis, or continued work in any
of the subtopics this course surveyed.

## 5. In-Class Exercise (Post-Presentations)

Each student writes one sentence connecting their own capstone topic to at least one other
presented capstone topic this week (a shared technique, a contrasting result, or a natural
follow-up experiment combining both).

# Week 11 Lecture Plan — Machine Learning (Graduate)
## Topic: Structured Prediction — Conditional Random Fields

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain structured prediction and why independent per-label prediction is insufficient. (*Understand*)
2. State the linear-chain CRF model and its partition function. (*Understand*)
3. Contrast CRFs (discriminative) with HMMs (generative) and explain the practical implications. (*Analyze*)
4. Fit a linear-chain CRF to a sequence-labeling task. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Structured prediction | Why joint sequence labeling beats independent per-token labeling |
| 0:15–0:45 | The CRF model | Feature functions, partition function, Viterbi-style decoding (conceptual) |
| 0:45–1:10 | Discriminative vs. generative | The CRF/HMM contrast; what each can and cannot model |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Training | Concave conditional log-likelihood; connection to Week 6's convex optimization |
| 1:45–2:00 | Scope note | Why this is not a general PGM-inference treatment (that's AI, Graduate) |

### Materials/Equipment
- Whiteboard for the CRF model equations
- Jupyter notebook with `sklearn-crfsuite` for a toy tagging task

### Formative Check (in-class)
Students give one feature a CRF could use easily that an HMM emission model would struggle to
represent.

### Link to Lab/Assessment
Lab 11: fit a linear-chain CRF to a small sequence-labeling task; inspect learned feature
weights. **Quiz 4** (Weeks 9–10) administered.

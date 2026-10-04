# Week 7 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: The Grokking Phenomenon

**Duration:** 2 hours lecture + 3 hour seminar (critical-writing/discussion format)

### Learning Objectives (Bloom's Level)
1. State Power et al.'s grokking observation precisely, including why it is theoretically
   puzzling. (*Understand*)
2. Critically evaluate the competing hypotheses for grokking against the available evidence.
   (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | The observation | Training accuracy saturates near-immediately; test accuracy stays at chance for a long stretch, then rises sharply |
| 0:20–0:40 | Why this is puzzling | Classical "train and test track once training loss is near zero" story directly violated; the delay can be very large |
| 0:40–1:15 | Competing hypothesis 1 | Slow implicit-regularization/weight-decay-driven transition from a memorizing to a generalizing solution |
| 1:15–1:50 | Competing hypothesis 2 | Circuit-formation account: a generalizing computational circuit slowly forms/strengthens alongside a memorizing one |
| 1:50–2:00 | Honest synthesis | What current evidence does and does not settle; role of dataset size and weight decay strength |

### Materials/Equipment
- Slides: "Grokking: Delayed Generalization"
- Handout: Power et al.'s central empirical figure (described, not reproduced) and a short summary
  of each competing hypothesis's claim and evidence

### Formative Check (in-class)
State, in your own words, why "training accuracy is already ~100%" and "the network has already
found a generalizing solution" are different claims, and why grokking is evidence they can be
separated by a large number of training steps.

### Link to Lab/Assessment
Lab 7: a critical-writing position paper on the competing grokking hypotheses (see
`lab-manuals/lab-07.md`). No required code this week; an optional, ungraded small reproduction is
suggested for students who want to see the effect directly.

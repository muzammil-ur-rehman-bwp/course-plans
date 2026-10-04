# Week 5 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Full-Information Online Convex Optimization

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the OCO protocol and regret, and distinguish full information from the bandit setting.
   (*Understand*)
2. Derive online gradient descent's $O(\sqrt T)$ regret bound. (*Apply, Analyze*)
3. Implement FTRL and OGD and empirically compare their regret on a synthetic loss sequence.
   (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | The OCO protocol | Full-information vs. bandit setting, explicit contrast with *Advanced AI* Week 2–3 |
| 0:15–0:40 | Follow-The-Regularized-Leader | Definition; intuition (regularizer prevents overfitting to early rounds) |
| 0:40–1:15 | Online gradient descent | Linearization of FTRL; projection step; regret-bound derivation |
| 1:15–1:45 | Optimizing the step size | Deriving $\eta_t=\Theta(1/\sqrt t)$ and the resulting $O(\sqrt T)$ bound |
| 1:45–2:00 | Synthesis | Recap table: protocol, update rule, regret bound; pointer to the sibling course's different setting |

### Materials/Equipment
- Slides: "Full-Information Online Convex Optimization: FTRL and OGD"
- Whiteboard for the projection/telescoping regret derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Explain in two sentences why the OCO learner's ability to see $f_t$ in full (not just $f_t(x_t)$)
is what makes a gradient-based update like OGD well-defined, while the bandit setting cannot use
$\nabla f_t(x_t)$ directly.

### Link to Lab/Assessment
Lab 5: implement OGD and FTRL and plot empirical regret against the $O(\sqrt T)$ bound (see
`lab-manuals/lab-05.md`).

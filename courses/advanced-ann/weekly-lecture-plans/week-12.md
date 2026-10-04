# Week 12 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: Loss-Landscape Geometry — Mode Connectivity; The Lottery Ticket Hypothesis Revisited

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain mode connectivity and what it does and does not imply about landscape structure.
   (*Understand, Analyze*)
2. Implement a linear-interpolation and a nonlinear bend-point path-finding experiment between two
   independently trained minima. (*Apply*)
3. Critically evaluate current refinements of and critiques of the Lottery Ticket Hypothesis,
   including the linear-mode-connectivity refinement. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | One-sentence pointer to the graduate course's Lottery Ticket Hypothesis result |
| 0:15–0:45 | Mode connectivity | Linear interpolation's loss barrier; nonlinear low-loss paths; what this implies (and doesn't) about the landscape |
| 0:45–1:15 | LTH revisited | Scale/learning-rate difficulties finding winning tickets reliably; the linear-mode-connectivity refinement |
| 1:15–1:45 | LTH critiqued | Open questions about reading pruning evidence as "already there at initialization" vs. an artifact of the procedure |
| 1:45–2:00 | Synthesis | Table: what LTH's original claim, its refinements, and its critiques each establish |

### Materials/Equipment
- Slides: "Mode Connectivity & the Lottery Ticket Hypothesis Revisited"
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Explain why a high linear-interpolation loss barrier between two minima does not, by itself, imply
those minima are not part of a single connected low-loss region of the landscape.

### Link to Lab/Assessment
Lab 12: implement linear interpolation and a bend-point nonlinear path between two independently
trained minima, and compare loss along each path (see `lab-manuals/lab-12.md`).

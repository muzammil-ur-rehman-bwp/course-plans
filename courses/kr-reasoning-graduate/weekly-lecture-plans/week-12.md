# Week 12 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Multi-Agent Epistemic Reasoning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the Kripke-model constructions for everyone-knows (E_G), common knowledge (C_G), and
   distributed knowledge (D_G). (*Understand*)
2. Construct the Kripke model for a small muddy-children instance and trace the public-
   announcement update round by round. (*Apply*)
3. Explain, on one worked example, how E_G, C_G, and D_G differ, and contrast this week's
   epistemic lens on multi-agent systems with *Artificial Intelligence* Graduate's
   game-theoretic lens. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 2's single-agent epistemic modal logic as the building block |
| 0:15–0:45 | Group knowledge | E_G (union of R_i), common knowledge C_G (transitive closure), distributed knowledge D_G (intersection of R_i) |
| 0:45–1:15 | The muddy children puzzle | Full worked statement and induction argument |
| 1:15–1:25 | Break | — |
| 1:25–1:55 | Simulation | Public-announcement world elimination, traced round by round |
| 1:55–2:00 | Contrast | Epistemic lens (what agents know) vs. *AI* Graduate's game-theoretic lens (strategic payoffs) |

### Materials/Equipment
- Slides: "Common and Distributed Knowledge; The Muddy Children Puzzle"
- Whiteboard for the induction argument
- Live-coding environment (Jupyter)

### Formative Check (in-class)
For a 3-child, 2-muddy instance, predict (before simulating) at which round the muddy children
will announce "yes," then verify with the provided simulation.

### Link to Lab/Assessment
Lab 12: implement the muddy-children public-announcement simulator and verify the round-k result
for several (n,k) instances (see `lab-manuals/lab-12.md`).
- **Quiz 5 this week** (Weeks 10–11 content).

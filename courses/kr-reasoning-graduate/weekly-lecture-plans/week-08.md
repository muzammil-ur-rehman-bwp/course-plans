# Week 8 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Argumentation Frameworks; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the definitions of conflict-free, admissible, grounded, and preferred sets for a Dung
   argumentation framework. (*Understand*)
2. Compute the grounded extension (via characteristic-function fixpoint) and a preferred
   extension of a small AF. (*Apply*)
3. Explain, on a mutual-attack example, why the grounded and preferred extensions can diverge.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Why a logic-level treatment of "conflicting arguments" needs its own abstraction |
| 0:15–0:45 | Dung's AF | Attack relation, conflict-free/admissible/acceptable-w.r.t.-S, worked by hand |
| 0:45–1:10 | Grounded extension | Characteristic-function fixpoint, worked on a 5-argument AF |
| 1:10–1:20 | Break | — |
| 1:20–1:40 | Preferred extensions | Maximal admissible sets; a mutual-attack example where grounded is empty but two preferred extensions exist |
| 1:40–2:00 | Midterm review | Practice problems spanning Weeks 1–8 |

### Materials/Equipment
- Slides: "Dung's Abstract Argumentation Frameworks"
- Whiteboard for fixpoint iteration and the odd-cycle example
- Midterm review problem set (handout)

### Formative Check (in-class)
Given a 5-argument AF's attack relation drawn as a graph, compute its grounded extension by
iterating the characteristic function by hand before checking with code.

### Link to Lab/Assessment
Lab 8: implement grounded- and preferred-extension computation for small AFs (see
`lab-manuals/lab-08.md`).
- **Quiz 3 this week** (Week 6 content). **Midterm Exam next week** (Weeks 1–8).

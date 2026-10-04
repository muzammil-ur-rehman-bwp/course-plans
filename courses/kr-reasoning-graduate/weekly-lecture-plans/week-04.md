# Week 4 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Description Logics in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Run the ALC tableau algorithm's completion rules (⊓, ⊔, ∃, ∀) by hand on a small concept.
   (*Apply*)
2. Determine, from the presence or absence of a clash, whether a concept is satisfiable.
   (*Analyze*)
3. State the PSPACE-completeness result for ALC satisfiability and match an application's
   expressiveness/tractability needs to the right OWL 2 profile (EL/QL/RL). (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Undergraduate DL basics (concepts, roles, ⊓, ¬, ∃R.C, ∀R.C) and the toy subsumption check |
| 0:15–0:55 | The tableau algorithm | Completion rules board-worked; a satisfiable example (open branch) and an unsatisfiable example (clash) |
| 0:55–1:05 | Break | — |
| 1:05–1:30 | Complexity | Why ALC satisfiability is PSPACE-complete; how more expressive DLs (e.g., SHIQ) push complexity to EXPTIME-complete |
| 1:30–1:55 | OWL 2 profiles | EL/QL/RL: what each trades away and gains, with one real-world use case per profile |
| 1:55–2:00 | Synthesis | Tableau methods as the bridge to Week 5's general FOL tableau |

### Materials/Equipment
- Slides: "The ALC Tableau Algorithm and DL Complexity"
- Whiteboard for the hand-worked tableau derivations
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a small ALC concept, predict (before running the algorithm) whether it will clash, then
run the tableau rules by hand to check.

### Link to Lab/Assessment
Lab 4: implement a tableau-based ALC satisfiability checker and test it on satisfiable and
unsatisfiable concepts (see `lab-manuals/lab-04.md`).
- **Quiz 2 next week** (this week's content).

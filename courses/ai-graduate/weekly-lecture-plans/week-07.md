# Week 7 Lecture Plan — Artificial Intelligence (Graduate)
## Topic: Rigorous Classical Planning

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. State the PSPACE-completeness result for classical planning and give the intuition for why
   it is believed strictly harder than NP-complete problems. (*Understand, Analyze*)
2. Build a planning graph by hand and read off a relaxed-plan heuristic value. (*Apply, Analyze*)
3. Implement a simple HTN decomposition for a toy domain. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | STRIPS representation review from the undergraduate course |
| 0:15–0:40 | Planning complexity | PLAN-SAT/plan existence is PSPACE-complete: statement and intuition |
| 0:40–1:15 | Planning graphs & relaxed heuristics | Board-worked planning-graph construction; ignoring delete lists |
| 1:15–1:45 | HTN planning | Task decomposition methods; why structure helps beyond flat STRIPS search |
| 1:45–2:00 | Synthesis | Capstone topic-selection discussion begins |

### Materials/Equipment
- Slides: "Planning Complexity, Heuristics, and HTN Decomposition"
- Whiteboard for the planning-graph construction example
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a small STRIPS domain, build the first two layers of its planning graph and compute the
relaxed-plan heuristic estimate for a given goal literal.

### Link to Lab/Assessment
Lab 7: build a relaxed planning graph and implement a small HTN decomposition
(see `lab-manuals/lab-07.md`). **Capstone topic selection opens** (informal proposals due to
instructor by end of Week 8).

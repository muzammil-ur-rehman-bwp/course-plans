# Week 1 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Introduction to Knowledge Representation

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Define knowledge representation and the KR hypothesis. (*Remember*)
2. Explain the three desiderata for a good representation — expressiveness, inferential
   efficiency, naturalness. (*Understand*)
3. Map a given piece of domain knowledge onto two or more of the representation schemes surveyed
   this semester and compare them against the desiderata. (*Understand, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Course overview | Syllabus walkthrough, assessment plan, how this course differs from *Introduction to AI*'s brief logic/planning/Bayes survey |
| 0:15–0:40 | What is KR? | The KR hypothesis; why representation is a first-class problem, not an afterthought to search |
| 0:40–1:05 | Desiderata | Expressiveness, inferential efficiency, naturalness; worked examples where each trades off against the others |
| 1:05–1:15 | Break | — |
| 1:15–1:45 | The map of the semester | Overview of logic, semantic networks, frames, rules, description logics; one small domain sketched under three schemes |
| 1:45–2:00 | Discussion | Why no single scheme wins on all three desiderata; preview of Weeks 2–7 |

### Materials/Equipment
- Slides: desiderata comparison table, the semester's representation-scheme map
- Python/Jupyter environment check (for Lab 1)

### Formative Check (in-class)
Exercise: in pairs, take a small domain (e.g., a university's course-and-prerequisite
information) and sketch how it would look as (a) a flat list of English sentences, (b) a set of
logical facts, (c) a graph of named relations; discuss which desideratum each version favors.

### Link to Lab/Assessment
Lab 1: Build a simple triple-store (subject, predicate, object) representation of a mini-world in
Python and compare it, on paper, against a flat-fact representation of the same world.

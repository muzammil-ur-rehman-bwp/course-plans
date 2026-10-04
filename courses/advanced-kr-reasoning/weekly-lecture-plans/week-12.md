# Week 12 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Ontology Evolution and Versioning

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Identify change-detection, impact-analysis, and backward-compatibility problems for evolving
   ontologies. (*Understand*)
2. Perform a manual impact analysis across two toy ontology versions. (*Apply, Analyze*)
3. Argue, with specific entailment evidence, whether a hypothetical change is backward-compatible.
   (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Motivation | Why deployed ontologies are rarely static; dependent-reasoning risk |
| 0:20–0:50 | Change detection & diffing | Syntactic vs. logically-equivalent restatements |
| 0:50–1:20 | Impact analysis | Entailments gained/lost between versions |
| 1:20–1:45 | Backward compatibility & modularity | Versioning policy choices; module-boundary limits |
| 1:45–2:00 | Synthesis | This week as a grounded survey, not a solved algorithm |

### Materials/Equipment
- Slides: "Ontology Evolution and Versioning"
- Two toy ontology versions (handout) for the in-class exercise

### Formative Check (in-class)
Given two small ontology versions, identify one entailment gained and one lost, and state
whether the change should be considered backward-compatible.

### Link to Lab/Assessment
Lab 12: build a simple ontology-diff tool reporting changed query answers across two versions
(see `lab-manuals/lab-12.md`). **Quiz 6** (Weeks 11–12 content) this week.

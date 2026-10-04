# Week 2 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Propositional Logic in Depth — Normal Forms, Resolution, SAT

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Convert a propositional sentence to CNF and DNF by the standard transformation steps.
   (*Apply*)
2. Apply the resolution rule to derive a resolvent from two clauses, and apply resolution
   refutation to prove or disprove entailment. (*Apply*)
3. Explain the Boolean satisfiability problem (SAT) and why its NP-completeness bounds how far
   resolution-based reasoning can scale. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Recap | Propositional syntax/semantics, truth tables (fast recap, assumed prior exposure) |
| 0:20–0:50 | Normal forms | CNF and DNF conversion steps, worked on 3 example sentences |
| 0:50–1:00 | Break | — |
| 1:00–1:35 | Resolution | The resolution rule, multiple worked derivations; resolution refutation as proof by contradiction |
| 1:35–2:00 | SAT & complexity | The SAT problem, NP-completeness (statement, not proof), implications for scalability |

### Materials/Equipment
- Slides: CNF/DNF conversion steps, resolution-derivation traces, SAT complexity summary
- Starter notebook: CNF converter and resolution-refutation skeleton

### Formative Check (in-class)
Exercise: given two clauses with exactly one complementary literal pair, resolve them by hand;
then given a 4-clause KB and a query, trace resolution refutation to completion on the board.

### Link to Lab/Assessment
Lab 2: Implement a CNF/DNF converter, a resolution-refutation prover, and a brute-force SAT
checker for small propositional knowledge bases.

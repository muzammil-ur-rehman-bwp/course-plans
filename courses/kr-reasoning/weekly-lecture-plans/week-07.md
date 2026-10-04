# Week 7 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Description Logics and Ontologies

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Read and write basic DL concept/role expressions in a small fragment (`ALC`-style).
   (*Understand, Apply*)
2. Explain the relationship between description logics and first-order logic, including the
   expressiveness/decidability trade-off. (*Understand, Analyze*)
3. Describe OWL/RDF and the Semantic Web vision, and implement a toy subsumption check for a
   small fixed DL fragment. (*Understand, Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why description logics? | Taxonomic knowledge, decidability vs. full FOL, the DL design philosophy |
| 0:25–0:55 | DL syntax | Concepts, roles, constructors (`⊓, ¬, ∃R.C, ∀R.C`) in a small fragment |
| 0:55–1:05 | Break | — |
| 1:05–1:30 | DL and FOL | Translating DL concepts to FOL formulas; subsumption and instance checking |
| 1:30–2:00 | OWL/RDF & Semantic Web | RDF triples, OWL as standardized DL, the Semantic Web vision, real tooling (Protégé, Pellet) as context |

### Materials/Equipment
- Slides: DL constructor table, DL-to-FOL translation examples, RDF triple diagram
- Starter notebook: toy subsumption checker skeleton

### Formative Check (in-class)
Exercise: translate `Parent ⊑ ∃hasChild.Person` to FOL by hand; discuss what it asserts and what
it does not.

### Link to Lab/Assessment
Lab 7: Implement a toy subsumption checker for a small fixed DL fragment and sketch an RDF triple
representation of a small ontology fragment.

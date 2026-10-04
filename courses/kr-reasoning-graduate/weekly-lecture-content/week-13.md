# Week 13 — Lecture Content: Ontology Engineering in Practice

## 1. From Algorithm to Practice
Week 4 gave the tableau algorithm that decides DL satisfiability. This week asks: how do real
teams actually *build* and *maintain* an ontology that a reasoner like that will be run against,
and how do two independently built ontologies get reconciled?

## 2. An Iterative Development Methodology
A practical ontology-development cycle (following the spirit of Noy & McGuinness, 2001):
1. **Specification.** Define scope and purpose via **competency questions** — concrete questions
   the ontology must be able to answer (e.g., "which employees work in a department located in a
   given city?"). Competency questions drive scoping decisions far better than an abstract
   "model the domain" goal.
2. **Conceptualization.** Sketch the key classes, relations, and constraints informally.
3. **Formalization.** Commit to a DL fragment/OWL profile (Week 4) and write the formal
   axioms.
4. **Implementation.** Encode in OWL, load into an editor (Protégé, §4), run a reasoner to check
   consistency and compute the classification hierarchy.
5. **Evaluation.** Check every competency question is actually answerable by querying the
   ontology; check for unintended inferences (a classic failure mode: an axiom that is locally
   reasonable but, combined with others, forces an unintended subsumption).
6. **Maintenance.** As requirements evolve, repeat the cycle — re-check competency questions and
   re-run the reasoner after every non-trivial change, since DL axioms interact non-locally.

**Why competency questions matter.** A class hierarchy that looks complete can still fail its
actual purpose if it cannot answer the specific questions the ontology was commissioned to
answer; writing them down first turns "is this ontology good?" into a checklist rather than a
matter of taste.

## 3. Ontology Alignment / Matching
Two independently built ontologies rarely use the same names or granularity for "the same"
real-world concepts (synonymy: `Employee` vs. `Staff`; granularity: one ontology's single
`Vehicle` class vs. another's `Car`/`Truck`/`Motorcycle` split). **Alignment** proposes
**correspondences** (most commonly equivalence or subsumption relations) between the two
ontologies' concepts, using signals such as:
- **Lexical/string-based similarity** — how similar the class labels are as strings or tokens.
- **Structural similarity** — how similar the candidates' positions are in their respective
  class hierarchies (shared superclasses/subclasses, sibling structure).
- **Instance-based similarity** — whether the two classes share individuals/instances in common
  data.

A simple, implementable lexical matcher using normalized token-Jaccard similarity:

```python
import re

def tokenize(label):
    # split CamelCase and snake_case/space-separated labels into lowercase tokens
    s = re.sub(r'(?<!^)(?=[A-Z])', '_', label)
    return set(t.lower() for t in re.split(r'[_\s]+', s) if t)

def jaccard(tokens1, tokens2):
    if not tokens1 and not tokens2:
        return 1.0
    return len(tokens1 & tokens2) / len(tokens1 | tokens2)

def propose_alignments(classes1, classes2, threshold=0.3):
    proposals = []
    for c1 in classes1:
        for c2 in classes2:
            score = jaccard(tokenize(c1), tokenize(c2))
            if score >= threshold:
                proposals.append((c1, c2, score))
    return sorted(proposals, key=lambda x: -x[2])
```

**Why alignment is hard in practice.** `Employee` and `Staff` have zero token overlap (lexical
similarity fails completely) despite being a near-perfect equivalence — only structural or
instance-based evidence (or a lexical resource such as a synonym list) would catch this.
Conversely, `Account` in a banking ontology and `Account` in a user-management ontology are
lexically identical but refer to entirely different concepts — a reminder that every proposed
correspondence needs a human audit, not blind acceptance of the highest-scoring match.

## 4. Real DL Reasoner Tooling
- **Pellet** and **HermiT** are tableau-based OWL DL reasoners — production implementations of
  (extensions of) the Week 4 tableau algorithm, scaled to real ontologies. They check
  **consistency** (does the ontology have a model at all — no concept forced to be both
  satisfiable and unsatisfiable by the axioms together), **classification** (compute the full
  subsumption hierarchy the axioms entail, which may include subsumptions the ontology engineer
  did not explicitly assert), and **instance checking** (does a given individual provably belong
  to a given class).
- **Protégé** is the standard ontology-editing environment; it calls a reasoner such as Pellet or
  HermiT on demand (or on save) and surfaces a detected inconsistency to the engineer directly in
  the editor — e.g., highlighting an unsatisfiable class in red, or showing an "explanation" view
  that traces which specific axioms together caused the inconsistency (a practical, UI-level
  analogue of Week 4's tableau clash trace).

## 5. In-Class/Lab Exercise
Using `propose_alignments`, align two toy class lists (e.g., `["ResearchPaper", "Author",
"Venue"]` vs. `["Publication", "Writer", "ConferenceVenue"]`) and manually audit: which proposed
correspondences are correct, which are missed because of zero lexical overlap (as with
`Employee`/`Staff` above), and which would need structural or instance evidence instead.

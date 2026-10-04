# Week 15 — Lecture Content: Current Trends in Knowledge Representation

## 1. Knowledge Graphs
A **knowledge graph** is, structurally, the Week 1 triple store and the Week 6 semantic network
scaled up and put into production: a large collection of `(subject, predicate, object)` triples
(a **triple store**) or, equivalently, a graph of typed nodes and labeled edges (a **property
graph**). Real organizations maintain internal knowledge graphs with millions or billions of
facts (e.g., linking products, people, organizations, and locations), and public triple-store-
backed knowledge bases make similarly structured data queryable at web scale — discussed here
conceptually, as the industrial-scale destination of the semantic-network ideas from Week 6,
rather than as a system this course builds.

```python
class KnowledgeGraph:
    def __init__(self):
        self.triples = set()

    def add(self, subject, predicate, obj):
        self.triples.add((subject, predicate, obj))

    def query(self, subject=None, predicate=None, obj=None):
        return {t for t in self.triples
                if (subject is None or t[0] == subject)
                and (predicate is None or t[1] == predicate)
                and (obj is None or t[2] == obj)}

    def path_query(self, start, pattern):
        """pattern: a list of predicates to follow in sequence, e.g. ['worksFor', 'locatedIn'].
        Returns the set of objects reachable from start by following each predicate in turn."""
        current = {start}
        for predicate in pattern:
            next_set = set()
            for node in current:
                next_set |= {t[2] for t in self.query(subject=node, predicate=predicate)}
            current = next_set
        return current
```

### Worked Example
```python
kg = KnowledgeGraph()
kg.add("alice", "worksFor", "acme")
kg.add("acme", "locatedIn", "springfield")
print(kg.path_query("alice", ["worksFor", "locatedIn"]))  # {"springfield"}
```
This answers "where is the organization Alice works for located?" by composing two one-hop
queries — exactly the kind of multi-hop pattern query real knowledge-graph systems answer at a
much larger scale, and structurally no different from a 2-hop walk over the Week 6 semantic
network's IS-A/part-of edges.

## 2. Neuro-Symbolic AI (Conceptual Survey)
**Neuro-symbolic AI** combines the symbolic representations and inference procedures this course
has built (logic, rules, frames, CSPs, Bayesian networks) with modern machine-learning
components, in either direction:
- **Learning feeds symbols:** a learned model (e.g., a text or image classifier) proposes
  candidate facts or rules, which a symbolic reasoner then checks for consistency, combines with
  existing knowledge, or uses in further inference — the learned component handles noisy,
  unstructured input; the symbolic component handles principled combination and explanation.
- **Symbols guide learning:** known logical constraints (e.g., "a valid schedule never
  double-books a room," straight from Week 8's CSP constraints) are used to filter, penalize, or
  guide a learned model's outputs, so the model's predictions are kept consistent with
  known structure rather than learned purely from data.

This is a survey-level discussion — no neuro-symbolic system is implemented in this course — but
it is worth stating plainly where this course's formalisms connect to the ML-centric pillar of
modern AI that `Introduction to Artificial Intelligence` surveys separately.

## 3. Evaluating KR Systems
Three standard axes for judging any knowledge representation and reasoning system, drawn on
implicitly throughout this course and made explicit here:
- **Correctness** — is the system's reasoning sound (never derives a false conclusion from true
  premises) and, ideally, complete (relative to a stated formal semantics) — exactly the
  soundness/completeness questions asked of resolution (Weeks 2, 4) and default logic (Week 9).
- **Scalability** — does the system's reasoning cost stay tractable as the knowledge base grows
  (e.g., AC-3's polynomial cost vs. SAT's worst-case exponential cost; variable elimination's
  factor-based savings vs. full enumeration)?
- **Explainability** — can the system justify *why* it reached a conclusion, as the Week 14
  derivation trace did, rather than only returning an opaque answer?

No system in this course maximizes all three simultaneously (the same three-way tension as
Week 1's expressiveness/efficiency/naturalness desiderata, now applied specifically to reasoning
quality) — a useful closing lens for judging any KR system encountered beyond this course,
including the capstone project.

## 4. In-Class Exercise
For the integrated agent built in Week 14, discuss which of the three evaluation axes its
derivation trace most directly strengthens, and propose one concrete way its scalability could be
tested as its rule base grows (e.g., timing `ask_forward` as the number of rules increases).

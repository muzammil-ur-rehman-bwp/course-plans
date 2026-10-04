# Week 1 — Lecture Content: Introduction to Knowledge Representation

## 1. What Is Knowledge Representation?
**Knowledge representation (KR)** is the sub-field of AI concerned with how facts about the world
are encoded inside a reasoning system so that the system can derive new facts from them. The **KR
hypothesis** (Brachman & Levesque) states that any system whose behavior we want to explain by
appeal to "what it knows" must have, somewhere inside it, a representation of that knowledge that
is (a) a structure we can point to, and (b) causally responsible for the system's behavior — the
system acts the way it does *because* of what is encoded in that structure, not by coincidence.

This matters because the introductory survey of AI treats logic, planning, and Bayesian networks
each as one week among many — useful for seeing the breadth of AI, but too brief to ask the
central question of this course: **given a fixed piece of knowledge, what is the best way to
write it down so a machine can use it correctly and efficiently?**

## 2. Desiderata for a Good Representation
Every representation scheme makes a three-way trade-off:

- **Expressiveness** — can the scheme say everything that needs to be said about the domain?
  (E.g., can it represent "some student has taken every course in the AI stream," which needs a
  nested quantifier?)
- **Inferential efficiency** — given the representation, how tractable is it to compute the
  conclusions we care about? (E.g., full first-order logic is highly expressive but, in general,
  undecidable to reason over; a restricted rule-based system is far less expressive but can answer
  queries in linear time.)
- **Naturalness** — does the representation map cleanly onto how a human domain expert thinks
  about the knowledge, making it easy to author, read, and debug? (E.g., "a penguin is a bird that
  cannot fly" reads naturally as an exception to a frame hierarchy, but awkwardly as a flat list of
  unrelated propositional facts.)

No scheme maximizes all three simultaneously. This course is organized around seeing exactly where
each scheme sits on this trade-off, and implementing the inference procedure that makes each
scheme actually usable.

## 3. The Map of This Semester
| Scheme | Expressiveness | Inferential efficiency | Naturalness | Covered |
|---|---|---|---|---|
| Propositional logic | Low | High (but SAT is NP-complete) | Low for relational facts | Week 2 |
| First-order logic | Very high | Low (undecidable in general) | Medium | Weeks 3–4 |
| Production rules | Medium | High (forward/backward chaining is efficient) | High for "if-then" expert knowledge | Week 5 |
| Semantic networks / frames | Medium | High for taxonomic queries | Very high for categories and exceptions | Week 6 |
| Description logics / ontologies | Medium (a decidable fragment of FOL) | Decidable, often efficient | High for structured domains | Week 7 |
| Constraint networks | Medium | High with propagation (AC-3) | High for "what's allowed" problems | Week 8 |
| Non-monotonic formalisms | Medium (adds defaults/exceptions) | Varies | High for common-sense exceptions | Week 9 |
| STRIPS / planning representations | Medium | Search-dependent | High for action/effect knowledge | Week 10 |
| Temporal/spatial formalisms | Medium | High for interval reasoning | High for "when"/"where" knowledge | Week 11 |
| Bayesian networks | Probabilistic, not logical | Exponential worst case, tractable via structure | High for causal/uncertain domains | Week 12 |

## 4. A Worked Example: One Domain, Three Schemes
Domain fact: *"Every student who has completed CS101 and MATH101 is eligible for AI301."*

- **Flat English sentence:** exactly as written above — natural, but a machine cannot act on it
  without parsing.
- **Propositional logic (one student, say `alice`):**
  `CompletedCS101_alice ∧ CompletedMATH101_alice → EligibleAI301_alice` — one sentence *per
  student*, since propositional logic has no variables. Expressive enough for one student, but not
  naturally expressive for "every student."
- **First-order logic:**
  `∀x (Completed(x, CS101) ∧ Completed(x, MATH101)) → Eligible(x, AI301)` — one sentence covers
  every student; this is exactly the expressiveness gap propositional logic cannot close, and it
  is why Weeks 3–4 exist.

## 5. A Minimal Python Representation to Start With
This course will build increasingly structured representations; Week 1's lab uses the simplest
possible one — a flat **triple store** — as a baseline every later scheme will be compared to.

```python
class TripleStore:
    def __init__(self):
        self.triples = []  # list of (subject, predicate, object) tuples

    def add(self, subject, predicate, obj):
        self.triples.append((subject, predicate, obj))

    def query(self, subject=None, predicate=None, obj=None):
        """Return all triples matching the given fields; None means 'any'."""
        return [
            t for t in self.triples
            if (subject is None or t[0] == subject)
            and (predicate is None or t[1] == predicate)
            and (obj is None or t[2] == obj)
        ]

kb = TripleStore()
kb.add("alice", "completed", "CS101")
kb.add("alice", "completed", "MATH101")
print(kb.query(subject="alice"))
```

This representation is maximally natural for simple facts and easy to query, but — as we will see
across the semester — it has no built-in notion of variables, quantification, inheritance, rules,
or uncertainty. Each later week adds exactly one such capability, in the order that makes each
addition's motivation clearest.

## 6. In-Class Exercise
For the domain "a company's employees, departments, and managers," sketch (on paper) how the fact
"every manager supervises at least one employee in their own department" would be written as (a) a
triple-store fact set, (b) a first-order logic sentence. Discuss which desideratum each favors.

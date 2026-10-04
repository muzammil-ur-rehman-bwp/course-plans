# Week 8 — Lecture Content: First-Order Logic; Midterm Review

## 1. Why Propositional Logic Is Not Enough
Propositional logic has no way to talk about **objects**, their **properties**, and the
**relations** between them without enumerating every case. "All students who study pass the
exam" cannot be written as a single propositional sentence for *every* student — we need
variables and quantifiers.

## 2. First-Order Logic Syntax
- **Constants** name specific objects: `Alice`, `Exam1`.
- **Predicates** express properties/relations: `Student(Alice)`, `Passes(Alice, Exam1)`.
- **Functions** map objects to objects: `MotherOf(Alice)`.
- **Variables** and **quantifiers**:
  - `∀x P(x)` — "for all x, P(x) holds" (universal quantification).
  - `∃x P(x)` — "there exists an x such that P(x) holds" (existential quantification).

A well-formed FOL sentence combines these with the propositional connectives from Week 6
(`¬, ∧, ∨, →, ↔`).

## 3. Semantics (Brief)
A model for FOL specifies a domain of objects and an interpretation that maps each constant to
an object, each predicate to a relation over the domain, and each function to a mapping on the
domain. `∀x P(x)` is true in a model if `P(x)` holds for every object `x` in the domain;
`∃x P(x)` is true if `P(x)` holds for at least one object.

## 4. Translating English to FOL
A systematic approach: identify the objects, the predicates/relations, and whether the sentence
is universal or existential.

| English | FOL |
|---|---|
| "All students who study pass the exam." | `∀x (Student(x) ∧ Studies(x)) → Passes(x, Exam)` |
| "Some cats are black." | `∃x Cat(x) ∧ Black(x)` |
| "Nobody passes without studying." | `∀x Passes(x, Exam) → Studies(x)` |
| "Every student has a favorite course." | `∀x ∃y Student(x) → Favorite(x, y)` |
| "There is one course every student likes." | `∃y ∀x Student(x) → Likes(x, y)` |

The last two rows are a classic trap: swapping quantifier order changes the meaning entirely —
"every student has *some* favorite course (possibly different per student)" versus "there is
*one* course that *all* students like."

## 5. A Simple Well-Formedness Checker (Illustrative)
We will not implement a full FOL parser/evaluator in this course (that is a significant
undertaking), but a small checker that confirms balanced quantifiers and parentheses in a
stringified sentence is a reasonable, honest illustration of syntax-checking:

```python
def quantifiers_balanced(sentence: str) -> bool:
    """Very small sanity check: every forall(x) / exists(x) must bind a variable
    that appears later in the sentence body."""
    import re
    bindings = re.findall(r"(forall|exists)\(([a-z])\)", sentence)
    body = re.sub(r"(forall|exists)\([a-z]\)", "", sentence)
    return all(var in body for _, var in bindings)

print(quantifiers_balanced("forall(x) (Student(x) and Studies(x)) -> Passes(x, Exam)"))  # True
print(quantifiers_balanced("forall(x) Passes(Exam)"))  # False: x never used in the body
```

This is a syntax sanity check only — it says nothing about semantics or truth, which is why the
course emphasizes doing translation exercises by hand and by discussion rather than relying on
code to "solve" FOL translation.

## 6. Midterm Review Roadmap
The midterm (Week 9) covers Weeks 1–8:
- Week 1: AI definitions, history, Turing Test, PEAS.
- Week 2: agent types, environment properties.
- Week 3: search problem formulation, BFS/DFS/UCS.
- Week 4: heuristics, A*, hill climbing, simulated annealing.
- Week 5: minimax, alpha-beta pruning.
- Week 6: propositional logic syntax/semantics, truth tables, equivalence.
- Week 7: entailment, CNF, resolution, forward/backward chaining.
- Week 8: FOL syntax/semantics, English-to-FOL translation.

## 7. In-Class Exercise
Translate 5 English sentences (including at least one with nested quantifiers) into FOL in
pairs; discuss disagreements as a class, paying special attention to quantifier order and scope.

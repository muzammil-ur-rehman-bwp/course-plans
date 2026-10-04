# Week 13 — Lecture Content: Research Methods for Advanced KR Research

## 1. Reading a Frontier KR Paper Efficiently
At the postgraduate level, a paper is read in a deliberate order, not front-to-back: **abstract**
(what is claimed) → **results/examples** (what is actually shown, and on what instances) →
**full formal definitions and proofs** (does the argument actually establish the claim, with what
assumptions) → **related work** (how the paper positions itself) → only then a full linear read.
This order front-loads the question that matters most for critique: *does the argument actually
establish what the abstract claims?*

## 2. A Structured Critique Framework
For any KR paper, ask explicitly:
- **Is the claimed formal property actually established?** A paper claiming soundness,
  completeness, or a complexity bound must either prove it or cite an existing proof — a claim
  stated without either is a conjecture, not a result, and should be read (and cited, if at all)
  as one.
- **Are the worked examples representative, or cherry-picked?** A single hand-picked example
  that happens to avoid a formalism's known hard cases does not support a general claim about
  that formalism's behavior.
- **How does the proposed formalism/algorithm relate to the alternatives covered in this
  course?** Does it extend SROIQ, ASPIC+, the distribution semantics, differentiable logic, model
  checking, or belief merging — and does it actually improve on, or merely restate, something
  this course already covers?
- **Reproducibility concerns specific to KR research:** is the proposed logic's semantics fully
  and unambiguously specified (every connective's truth conditions given, not left to the
  reader's inference)? Are complexity claims proved, or only conjectured? Is example code or a
  reference encoding available for an implementation-heavy claim?

## 3. Formulating a Research Problem Statement
A problem statement names a **precise gap**, not a topic. The test: could a knowledgeable reader,
after reading only the statement, state what evidence would resolve it? "I want to study
probabilistic description logics" is a topic. "Does attaching ProbLog-style independent
probabilistic facts directly to SROIQ role assertions preserve N2ExpTime-decidability, or does
the resulting formalism's distribution semantics require giving up decidability to retain
SROIQ's full nominal/number-restriction expressiveness?" is a problem statement — a knowledgeable
reader instantly sees what would resolve it: either a decidability proof, or a specific
undecidability reduction. This precision test is applied, repeatedly and specifically, to each
student's own capstone direction this week.

## 4. Related-Work Survey Standards
A survey of 5+ papers must: (1) accurately represent **what each paper actually claims and
found** — not a paraphrase of its title or abstract; (2) **relate the papers to each other** — who
builds on whom, where they agree or conflict, not five isolated summaries; (3) **end by using the
survey to motivate the specific gap** named in the problem statement. A survey that is accurate
on every individual paper but never says how they relate, or never connects back to the stated
gap, has not met this standard regardless of how carefully each individual summary was written.

## 5. Capstone Work Time
Structured, instructor-guided time this week: refine a draft problem statement via peer critique
(applying the precision test directly, out loud, to a peer's draft) and annotate at least 5
candidate related-work papers with each paper's precise claim and finding (not its abstract,
paraphrased).

## 6. In-Class/Lab Exercise
Given a short excerpt from a real KR paper, apply the structured critique framework explicitly:
state the paper's central claim, assess whether the excerpt's argument (as given) actually
establishes it, name one concrete reproducibility concern, and state which course topic the
excerpt most directly builds on or conflicts with. Then draft (or revise) your own capstone
problem statement and subject it to the precision test with a partner, revising until your
partner can state, from the statement alone, what evidence would resolve it.

# Lab Manual 8 — First-Order Logic: Translation & Well-Formedness

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Translate English sentences to FOL (paper exercise) and implement a simple well-formedness
checker for FOL sentence strings in Python.

## Setup
Create `lab08.ipynb` (for Task C–D) plus a markdown/paper section for Tasks A–B.

## Procedure
1. **Task A — Translation:** translate 5 instructor-provided English sentences into FOL,
   including at least one with nested quantifiers (`∀x ∃y ...` or `∃y ∀x ...`); write both the
   FOL sentence and a one-sentence justification for your choice of quantifier order.
2. **Task B — Back-translation:** given 3 FOL sentences (provided by the instructor), translate
   them back into clear English and flag any sentence you find ambiguous as written.
3. **Task C — Well-formedness checker:** implement `quantifiers_balanced(sentence)` as shown in
   the lecture content (or an improved version); test it on 3 well-formed and 3 deliberately
   malformed sentence strings.
4. **Task D — Reflection:** in 2–3 sentences, explain one case where the checker from Task C
   would pass a sentence that is syntactically balanced but semantically nonsensical, and why a
   full FOL parser would need more than this balance check.

## Expected Output
A short write-up for Tasks A–B; a notebook with Tasks C–D; the checker must correctly
distinguish well-formed from malformed test sentences.

## Submission
Submit `lab08.ipynb` plus the Task A–B write-up (markdown file or notebook cells) by the end of
the lab session.

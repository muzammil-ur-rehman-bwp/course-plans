# Assignment 2 — Structured/Quantitative Reasoning and Neuro-Symbolic Integration (Weeks 5–8)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 8 |
**Due:** Start of Week 10

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(ASPIC+, 20 pts)** Implement `build_arguments` and `attacks` exactly as in the Week 5
   lecture content. Given premises `{suspect_present, no_alibi}`, a defeasible rule
   `suspect_present, no_alibi ⇒ guilty`, and a second defeasible rule
   `has_alibi_witness ⇒ not_appl(suspect_present,no_alibi⇒guilty)` triggered by an added premise
   `has_alibi_witness`, construct the relevant arguments and confirm the second rule produces an
   **undercutting**, not a rebutting, attack. State in 2–3 sentences why undercutting is the
   correct attack type for this scenario rather than rebutting.
2. **(Distribution semantics, 20 pts)** Implement `least_model` and `distribution_semantics`
   exactly as in the Week 6 lecture content. For a program with three probabilistic facts
   `0.7::sensor_a`, `0.6::sensor_b`, `0.2::sensor_fault`, and rules
   `alarm :- sensor_a.`, `alarm :- sensor_b.`, `false_alarm :- alarm, sensor_fault.`, compute
   `P(alarm)` and `P(false_alarm)` both by hand (show your total-choice reasoning) and in code.
3. **(Differentiable logic, 25 pts)** Implement the product t-norm/t-conorm and the
   `constraint_loss` function from Week 7 in PyTorch. Given a batch of 10 learnable truth-degree
   pairs, train to satisfy the soft constraint `∀x. P(x) → Q(x)` for 100 steps; plot loss vs. step.
   Then repeat using the Gödel t-conorm-based implication; plot both curves on one axis and
   explain the difference in 3–5 sentences, citing the gradient argument from lecture.
4. **(Neuro-symbolic filtering, 20 pts)** Implement `filter_candidates` and at least two
   constraint functions from Week 8. Using a provided toy set of 10 ranked candidate triples (5
   genuinely valid, 5 violating at least one constraint), report which candidates survive and
   compare against the ground-truth validity labels provided; report precision of the top-3
   candidates before and after filtering.
5. **(Synthesis, 15 pts)** In 200–300 words, compare ASPIC+'s structured arguments, the
   distribution semantics, and differentiable/fuzzy logic as three different ways of adding
   structure or gradation to classical symbolic reasoning — state, for each, what classical
   ingredient it keeps unchanged and what it relaxes or restructures.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).

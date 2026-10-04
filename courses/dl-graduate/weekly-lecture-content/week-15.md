# Week 15 — Lecture Content: Research Project Work Session

## 1. Structure of This Week

This week has no new technical content; it is structured work time plus two presentation events:
the **Paper Critique & Presentation** assignment's in-class talks, and **capstone practice talks**
with structured peer feedback, ahead of Week 16's final capstone presentations.

## 2. Finalizing the Literature Review

A literature review (3–5 papers) is not a list of summaries — it must end with an explicit,
stated gap or question that the capstone's experiment addresses. For each paper, apply Week 14's
checklist (claim, evidence, baselines, ablations, reproducibility) and extract:
- The paper's central claim and method, in 2–3 sentences.
- How it relates to the other papers in the review (agreement, disagreement, a gap neither
  addresses).
- One specific limitation or open question — ideally the one the capstone experiment will probe.

## 3. Designing/Running the Experiment

For a **reproduced** experiment: identify the smallest version of the original experiment that
still tests the paper's central claim (e.g., a smaller model, dataset, or fewer training steps
than the original paper, if full-scale reproduction is infeasible), and state explicitly what was
scaled down and why that is unlikely to invalidate the comparison. For an **extended** experiment
(a variation/ablation on a reviewed paper's method): hold everything else fixed except the one
factor being varied (e.g., GCN vs. GAT aggregation on the same graph/task; DQN with vs. without a
target network on the same toy environment), and report results across **multiple random seeds**
where the method is stochastic — a single run's numbers are not evidence of a real difference,
exactly as emphasized in the sibling graduate AI course's research-methods week.

## 4. Peer Feedback Practice

Each practice talk should receive feedback addressing specifically:
- **Problem clarity:** could a listener who has not read the papers restate the motivating
  question after the talk?
- **Experiment soundness:** is there a clear baseline/comparison, and is the comparison fair
  (same compute budget, same data, controlled factors)?
- **Honesty about limitations:** does the talk acknowledge what did not work or what remains
  uncertain, rather than only presenting a clean positive result?

## 5. Paper Critique & Presentation (In-Class This Week)

See `assignments/assignment-04.md` for the full assignment. Presentations this week are 5 minutes
+ 2 minutes Q&A per student, covering the chosen paper's claim, evidence, and the presenter's own
assessment of its limitations.

## 6. In-Class Exercise

Trade capstone one-paragraph problem statements with a partner; each partner restates the other's
motivating question from memory after reading it once, and flags anything that was unclear or
over-broad (e.g., "study generalization" rather than a concrete, testable question).

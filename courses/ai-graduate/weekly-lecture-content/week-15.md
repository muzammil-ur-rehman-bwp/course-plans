# Week 15 — Lecture Content: Research Project Work Session

This week has no new technical content; it is a structured, instructor-guided work session for
the Research Capstone, applying everything from Week 13 (research methods) to each student's own
project. The content below is the guidance delivered during the session, not a new topic.

## 1. Finalizing the Literature Review
By this point, each student/pair should have 3–5 papers selected (from Week 7–8's topic
selection, refined through the term) relevant to their chosen capstone subtopic. The literature
review should, for each paper, state: (a) what problem it addresses, (b) its core method in one
or two sentences, (c) its reported result, and (d) how it relates to the student's own planned
experiment (a direct basis for reproduction, a point of comparison, or motivating context). The
review should end with an explicit statement of the gap or question the student's own experiment
addresses — this is the single most common weak point in student literature reviews: a list of
summaries with no synthesis or stated gap.

## 2. Finalizing the Experiment Design
Apply Week 13's experimental-design checklist directly to the capstone experiment:
- **Benchmark/task:** what exactly is being measured, and on what problem instance(s)?
- **Baseline:** what is the experiment being compared against (a simpler algorithm from this
  course, a default/unmodified version of the technique being extended, or a published reported
  number from one of the reviewed papers)?
- **Ablation (if applicable):** if the experiment proposes a modification or extension to an
  existing technique, what ablation isolates the modification's actual contribution?
- **Multiple runs:** for any experiment involving randomness (Q-learning's ε-greedy exploration,
  simulated annealing, genetic algorithms, random SAT instance generation, sampling-based
  inference), the plan must specify running multiple random seeds and reporting a measure of
  spread, not a single run's numbers — directly applying Week 13's statistical-significance
  guidance.

## 3. Giving and Receiving Structured Peer Feedback
Using the Week 15 peer-feedback worksheet, each practice talk is evaluated on three specific
axes, not a vague "good/bad" impression:
1. **Clarity of problem statement** — could a listener who has not seen the project before state
   back, in their own words, what problem is being addressed and why it matters?
2. **Soundness of experiment** — does the described experiment actually test the claim being
   made, with an appropriate baseline and (if relevant) ablation?
3. **Honesty about limitations** — does the presenter acknowledge what their experiment does
   *not* show, or what could have gone better, rather than overstating the result?
Peer feedback should give one specific, actionable comment on each axis — "good job" is not
useful feedback; "your problem statement didn't mention why multi-agent non-stationarity matters
for your specific experiment" is.

## 4. Revising in Response to Feedback
The practice-talk/peer-feedback round exists specifically so that weaknesses surface *before*
the graded Week 16 presentation, not during it. Students should treat the remainder of this
week's work session as time to act on the feedback received: tightening an unclear problem
statement, adding a missing baseline or ablation, or softening an overstated claim into an
honestly-scoped one — all of which directly improve the Week 16 presentation and final paper
grade per the capstone rubric (`assignments/capstone-rubric.md`).

## 5. Work-Session Deliverable
By the end of this week, each student/pair submits a capstone written-paper **draft** (need not
be final/polished, but must include a stated problem, a literature review with a stated gap, an
experiment design or preliminary results, and at least one acknowledged limitation) and the
peer-feedback worksheet completed for a classmate's practice talk.

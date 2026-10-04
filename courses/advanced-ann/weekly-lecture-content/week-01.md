# Week 1 — Lecture Content: The Neural-Network-Theory Research Frontier

## 1. What This Course Assumes (Stated, Not Re-Taught)
This course assumes the graduate *Artificial Neural Network* course as settled background. None
of the following is re-derived here; each is listed once, with the week of this course that
revisits it in depth noted where relevant:
- **Universal approximation and expressivity** (single-hidden-layer approximation theorem,
  depth-vs-width tradeoffs) — not revisited; assumed throughout any expressivity remark.
- **Automatic differentiation formalism** (forward-/reverse-mode AD, backprop as a special case)
  — assumed; every derivative computed in this course is either hand-derived or produced by
  PyTorch's autograd, never re-explained at the mechanics level.
- **Initialization theory** (Xavier/Glorot, He, variance-preservation arguments) — assumed, and
  revisited as the *linear, small-signal special case* of this course's Week 3 mean-field signal-
  propagation analysis.
- **Normalization theory** (BatchNorm/LayerNorm derivations) — assumed; referenced only in passing
  when relevant to sharpness (Week 6) or scaling (Week 8) discussions.
- **Optimization-landscape theory** (saddle points dominating bad local minima, second-order
  methods, adaptive optimizers) — assumed; the random-matrix/spin-glass argument behind the
  saddle-point claim is revisited at its *statistical-physics source* in Week 9.
- **Classical generalization theory** (PAC learning, VC dimension, Rademacher complexity, and a
  conceptual introduction to double descent) — assumed; VC/Rademacher's practical vacuity is
  revisited and contrasted with PAC-Bayes in Week 11, and double descent is revisited rigorously
  across three axes in Week 10.
- **A first survey of NTK, the Lottery Ticket Hypothesis, and the information bottleneck idea** —
  assumed at survey depth; NTK is revisited with a real derivation sketch in Week 2, and the
  Lottery Ticket Hypothesis is revisited with current refinements and critiques in Week 12.

## 2. Why "Revisited in Depth" Rather Than "New Topic"
A fair question: if the graduate course already covered NTK and the Lottery Ticket Hypothesis, why
does this course return to them? Because a survey-level treatment and a research-depth treatment
answer different questions. The graduate course's NTK week establishes *that* the infinite-width
limit yields kernel regression against a fixed kernel, and states the feature-learning limitation
as a conclusion. This course's Week 2 instead works through *why* the limit holds (the Taylor-
linearization argument), names the precise regime ("lazy training") in which it is valid, and
subjects the "no feature learning" limitation to a sharper, current-literature critique. The same
pattern holds for the Lottery Ticket Hypothesis in Week 12: the graduate course establishes the
original claim; this course surveys what the subsequent literature has found when researchers
tried to push the claim further (scale, learning rate, linear mode connectivity).

## 3. The Open-Problems Landscape This Course Covers
| Topic | Builds on (graduate course) | Pushes into |
|---|---|---|
| NTK revisited (Wk 2) | NTK survey-level statement | Infinite-width derivation sketch; lazy training; sharper critique |
| Mean-field theory (Wk 3) | Initialization/signal-propagation theory | The mean-field width→∞ limit; contrast with NTK |
| Implicit bias (Wk 4) | Optimization-landscape theory | Max-margin GD result, derived; nonlinear-setting survey |
| Feature learning (Wk 5) | NTK survey-level statement | How/why finite-width escapes the lazy regime |
| Sharpness & SAM (Wk 6) | Optimization-landscape theory | Flat/sharp minima; SAM as a concrete method |
| Grokking (Wk 7) | (new) | Delayed generalization; competing hypotheses |
| Scaling laws (Wk 8) | (new) | Power-law form; theoretical explanation attempts |
| Statistical physics (Wk 9) | Saddle-point/random-matrix argument | Replica method; spin-glass analogies, at their source |
| Double descent revisited (Wk 10) | Double descent, conceptual | Rigorous treatment across 3 axes; theoretical explanations |
| PAC-Bayes (Wk 11) | PAC/VC/Rademacher theory | Why classical bounds are vacuous; PAC-Bayes as a fix |
| Mode connectivity & LTH revisited (Wk 12) | Lottery Ticket Hypothesis survey | Mode connectivity; current LTH refinements/critiques |

## 4. Scoping Against the Sibling Postgraduate Courses
*Advanced Machine Learning* owns classical statistical-ML theory at postgraduate depth; *Advanced
Deep Learning* owns deep architectures at postgraduate depth; *Advanced Knowledge Representation
and Reasoning* owns deep KR formalisms. Where one of those is relevant to an argument here (for
example, a feature-learning argument that happens to use a convolutional architecture as its
example network), this course uses it only as an example and gives at most a one-sentence pointer
to the sibling course that owns that architecture or formalism in depth — never a lecture.

## 5. How to Scope a Research Proposal (Previewed)
This course's capstone (40% of the grade) is a research *proposal*, judged the way a thesis-
proposal committee judges a plan for research not yet completed, not a finished project. Four
components, previewed now and treated in full in Week 13:
1. **Problem statement** — precise and falsifiable, not a topic ("I want to study grokking" is a
   topic; "does grokking's onset time, for a fixed architecture, scale predictably with training-
   set size across a range tractable to test at course scale?" is closer to a problem statement).
2. **Related-work survey (5+ papers)** — synthesized, not listed: how the papers relate, where
   they agree or conflict, and what specific gap the survey, taken together, leaves open.
3. **Proposed novel approach** — the student's own formulation, clearly distinguishable from
   restating any one surveyed paper's contribution.
4. **Feasibility argument or preliminary results** — what must be true for the approach to work,
   the main technical risk named honestly, and why it is judged manageable; or a small honestly-
   reported pilot.

## 6. In-Class/Lab Exercise
Complete the self-assessment checklist against the graduate-course prerequisites (identify any
topic you are not confident recalling, and flag it for independent review this week — this course
does not re-teach it). Then skim the instructor-provided landscape map and draft a 150–250 word
paragraph naming which 1–2 of this course's topics currently interest you most and why, as a true
first draft toward eventually scoping a capstone problem statement (see `lab-manuals/lab-01.md`).

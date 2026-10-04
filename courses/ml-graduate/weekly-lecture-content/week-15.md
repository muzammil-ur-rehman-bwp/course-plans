# Week 15 — Lecture Content: Research Methods and Project Work Session

## 1. How to Read a Theoretical ML Paper
A theory-oriented ML paper typically makes a precise claim of the form "under assumptions
$\mathcal{A}$, quantity $Q$ satisfies bound/property $P$." Reading it critically means answering,
explicitly, for every main result:
- **What exactly is being claimed?** Write the formal statement out in your own notation, matching
  it term-by-term against the paper's theorem statement. If you cannot restate it precisely, you
  do not yet understand what is claimed.
- **What assumptions does it depend on?** (e.g., boundedness, independence, realizability,
  convexity, a specific noise model). This course's Weeks 2–5 are full of such assumptions — a
  bound derived under i.i.d. sampling, say, says nothing valid under distribution shift.
- **Is the bound vacuous at the scale the paper applies it to?** (Recall Week 3's observation that
  a VC bound with $\mathrm{VCdim}(H)\gg m$ gives a right-hand side exceeding $1$, hence no real
  information — a classic way a technically correct bound can still be practically useless.)
- **Does the empirical evidence (if any) actually test the claimed regime,** or a more favorable
  special case?
- **What is the proof's key step**, and could you reproduce a simplified version of it?

## 2. Reproducibility Practices
For any theory-adjacent empirical claim (an experiment illustrating or stress-testing a bound or
derivation), good practice means reporting: the exact assumptions tested (and which, if any, were
deliberately violated to probe robustness); random seeds and the number of independent trials
(single anecdotal runs are not evidence for a probabilistic claim); the gap between a small,
illustrative toy experiment (as in most of this course's weekly code) and the paper's full claimed
generality; and any hyperparameters or implementation choices that could materially change the
result. These are exactly the practices the capstone (Week 16) and the Paper Critique assignment
are graded on.

## 3. Structured Capstone Work Session
This week's lecture slot is reserved for guided, in-class capstone work: students bring their
chosen statistical-learning-theory subtopic, discuss their literature review's 3–5 papers with the
instructor, and get feedback on the design of their reproduced-or-extended experiment or
derivation before the Week 16 presentation. See `assignments/capstone-proposal-guidelines.md` and
`assignments/capstone-rubric.md` for the full requirements.

## 4. Deliverable Due This Week: Paper Critique & Presentation
Students submit a written critique of a chosen statistical-learning-theory paper (see
`assignments/assignment-04.md` — the Paper Critique & Presentation assignment — for the full
suggested-topics list and grading rubric) and give a short in-class presentation applying Section
1's reading framework to their chosen paper.

## 5. In-Class Exercise
Using Section 1's five questions, critique the *stated generality* of this course's own Week 3 VC
bound: for what range of $m$ relative to $\mathrm{VCdim}(H)$ does it actually provide useful
(non-vacuous) information, and at what point does it become uninformative? Relate your answer to
Week 9 (ANN-Graduate)'s discussion of why this exact bound becomes vacuous for deep networks.

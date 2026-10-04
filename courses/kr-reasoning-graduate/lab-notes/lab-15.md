# Lab Notes 15 — Capstone Work Session

**Concept recap:** this session applies Week 15's research-methods framework to each student's
own capstone — a sound literature review ends in an explicit gap; a sound experiment reports
results honestly, including negative/partial ones; feedback should be specific and actionable.

**Common pitfalls:**
- **A literature review that is just a list of summaries**: per the capstone rubric, the review
  must end with a stated gap/question the experiment addresses — a review that stops at "here
  are 4 papers" without that closing synthesis is incomplete even if each summary is accurate.
- **Reporting only a single run of a stochastic method**: any experiment with randomness (e.g.,
  a TransE training run, a randomized matcher threshold sweep) needs multiple seeds/trials
  reported with their spread, not one cherry-pickable number — this is a direct extension of
  Week 15's reproducibility discussion to the student's own work.
- **Vague peer feedback**: "it was good" or "needs work" are not actionable — Task D's feedback
  should name a specific slide/claim/result and what would make it stronger.
- Treating a negative or partial result as something to hide rather than analyze — per the
  capstone rubric, honest analysis of why an experiment did not fully work is scored on its own
  merits, not penalized relative to an impressive-looking but less rigorously examined result.

**Debugging tip:** if the experiment's results look "too good" or suspiciously clean, re-check
for a data leak (e.g., a held-out test triple that leaked into training in a knowledge-graph
capstone) before writing it up — an implausibly strong result is itself a signal to double-check.

**Instructor tip:** rotate practice-talk groups so students give feedback to peers working on a
different subtopic than their own — explaining an unfamiliar formalism back to its presenter in
one's own words is often the fastest way to find a genuine gap in the presenter's explanation.

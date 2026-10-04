# Week 13 — Lecture Content: Research Methods for Applied Deep Learning Research

## 1. How to Read a Systems-and-Methods DL Paper Efficiently
A DL systems-and-methods paper's claims are usually quantitative and operational (a latency
number, a throughput number, an accuracy delta) under a *specific* hardware and configuration —
reading it critically means extracting exactly what was measured, not just the headline number:
1. **The precise claim.** What quantity, under what stated configuration (hardware, batch size,
   precision, sequence length)? A "2× speedup" claim is meaningless without knowing 2× relative to
   what baseline, measured how.
2. **Baselines and ablations.** Is the comparison baseline current and fairly tuned for its own
   configuration (an undertuned or outdated baseline inflates the apparent improvement)? Does an
   ablation isolate which specific design choice drives the reported gain, or could the same gain
   plausibly come from an unrelated change bundled into the same experiment?
3. **Reproducibility.** Could a reader with reasonable (not necessarily identical) resources
   reproduce the central claim? What is withheld (exact hyperparameters, exact hardware,
   training data details) that would make reproduction harder than it looks?

## 2. Benchmark Culture
Three specific, recurring failure modes are worth naming precisely, since "the paper uses a
benchmark" is not itself a red flag — these specific patterns are:
- **Leaderboard-chasing.** Optimizing a method specifically to climb a fixed benchmark's
  leaderboard, sometimes via choices (extensive benchmark-specific tuning, test-set-adjacent
  validation practices) that would not generalize to the benchmark's intended real-world proxy
  task.
- **Benchmark saturation/contamination.** A benchmark that is at or near its achievable ceiling
  stops meaningfully discriminating between methods; contamination (benchmark data leaking into
  pretraining data) can inflate reported performance in a way that has nothing to do with the
  capability the benchmark was designed to measure.
- **The benchmark-vs-deployment gap.** Strong benchmark performance does not automatically imply
  strong real-world robustness — a benchmark is, at best, a proxy for the capability someone
  actually cares about, and the gap between proxy and target can be large and underexamined.

## 3. Reproducibility Challenges Specific to Deep Learning
Beyond benchmark culture, DL research has reproducibility obstacles largely distinct from other
empirical sciences:
- **Compute cost as a barrier.** A paper's central result may require compute resources far
  beyond what most readers (including other researchers) can access, making independent
  verification rare in practice — a different situation from a field where any lab can rerun a
  cheap experiment.
- **Hyperparameter and hardware sensitivity.** A reported gain that depends on an
  under-disclosed batch size, learning-rate schedule, precision setting (recall Week 8's
  quantization material — results can be precision-sensitive in ways that are easy to omit), or
  specific hardware generation is fragile in a way a reader cannot detect from the paper's
  headline claim alone.

## 4. Applying the Framework
This week's graded exercise is identical in spirit to the critique framework used in the sibling
postgraduate courses (claim, evidence, baseline/ablation fairness, reproducibility concern,
course-pillar connection, overall assessment), adapted here specifically to systems-and-methods DL
claims rather than theoretical or alignment-research claims. The same structured standard from
Week 5 ("what was shown, under what assumptions, what gap remains to the general claim") applies
directly: a systems paper's "2× faster, no quality loss" claim should be read with the same
discipline as Week 5's induction-heads-vs-implicit-gradient-descent distinction.

## 5. Capstone Problem-Statement Drafting (Precision Test)
Apply the same precision test used across the postgraduate sequence: could a knowledgeable reader
state, after reading only your problem statement, what evidence would resolve it? "I want to study
Mixture-of-Experts load balancing" is a topic, not a problem statement. "Does replacing the
standard coefficient-of-variation auxiliary loss with a hard capacity-constraint-based routing
rule reduce the specific router-collapse pattern observed when $N\gg k$, without the accuracy cost
a hard capacity constraint is generally assumed to impose?" is a problem statement — specific
enough that a reader knows exactly what experiment would settle it.

## 6. In-Class/Lab Exercise
Given a short instructor-provided excerpt from a current DL systems-and-methods paper, write a
structured critique (claim, baseline/ablation fairness, one concrete reproducibility concern) in
150–200 words. Separately, begin the capstone literature search: identify 3–5 candidate papers
connected to one of this course's topics, and draft a one-paragraph problem statement for your own
candidate capstone topic, subjecting it to the precision test via peer feedback.

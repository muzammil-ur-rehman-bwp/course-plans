# Week 14 — Lecture Content: Research Methods in Deep Learning; Capstone Work Time

## 1. Reading a Paper Efficiently

A standard efficient reading order for a research paper: **abstract** (the claim, in miniature) →
**figures and results tables** (what was actually measured) → **method** (how) → **related work**
(what it claims to improve on) → full read of the remaining sections only if the above justify the
time investment. This order front-loads the two questions that determine whether deeper reading is
worthwhile: *what is being claimed*, and *is there evidence for it*.

## 2. Critiquing a Paper: A Structured Checklist

1. **Claim:** state the paper's central claim as a single, precise, falsifiable sentence.
2. **Evidence:** does the evidence (experiments, proofs, ablations) actually support that exact
   claim, or a narrower/different one?
3. **Baselines:** are the baselines compared against fair and current for the paper's
   publication venue/year, or outdated/weak comparators that flatter the proposed method?
4. **Ablations:** does the paper isolate *which part* of its proposed method drives the reported
   improvement, or does it only report the full method against a very different baseline?
5. **Reproducibility:** could another researcher, with reasonable effort, reproduce the central
   result from what is reported?

## 3. Reproducibility Challenges Specific to Deep Learning

- **Compute cost as a reproduction barrier.** Many deep learning results, especially at
  foundation-model scale (Week 13), require compute budgets far beyond most labs' means, making
  independent reproduction of the *exact* reported result infeasible even when the method and
  code are fully disclosed — a reproducibility barrier that has no direct analogue in, say,
  classical algorithms research.
- **Hyperparameter sensitivity.** A reported result can depend sensitively on an
  under-disclosed hyperparameter choice (learning rate schedule, random seed, data augmentation
  details, or even exact batch size); without full disclosure, a result's claimed effect may not
  reproduce under a different but reasonable hyperparameter setting, and attributing the gap to
  "a different implementation" versus "the original result not being robust" is often genuinely
  hard to tell apart.
- **Benchmark-culture critiques.** Leaderboard-driven research can incentivize chasing a single
  benchmark number (sometimes via benchmark-specific tricks, or via train/test contamination)
  rather than a genuinely more capable or robust method; a model's state-of-the-art benchmark
  score does not always predict its real-world robustness or generalization outside that
  benchmark's specific distribution.

## 4. Structured In-Class Critique Exercise

In small groups, apply the Section 2 checklist to a short, instructor-provided deep learning
paper excerpt, explicitly flagging: the claim; one piece of evidence that does (or does not)
support it; and at least one of the three DL-specific reproducibility concerns from Section 3
that applies to this excerpt.

## 5. Capstone Work Time: Finalizing Topic and Literature List

Using Weeks 1–13's topics as a menu, finalize an individual or paired capstone topic (see
`assignments/capstone-proposal-guidelines.md`) and assemble an initial list of 3–5 candidate
papers, applying this week's efficient-reading order to each candidate to decide quickly whether
it belongs on the final reading list.

## 6. In-Class Exercise

Pick one paper from your own candidate capstone reading list and, using only its abstract and
results figures/tables (not the full paper), write down your best current guess at its central
claim — then, after reading the method section, check whether your guess was precise enough.

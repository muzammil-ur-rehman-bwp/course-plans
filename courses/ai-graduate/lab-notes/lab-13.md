# Lab Notes 13 — Structured Paper Critique & Ablation Design

**Concept recap:** a structured critique separates claim, evidence, baseline adequacy,
ablations, and reproducibility into distinct, checkable questions; an ablation study isolates
one component's contribution by varying it while holding everything else fixed.

**Common pitfalls:**
- Restating the paper's abstract as the "claim" without distilling it to the specific, checkable
  assertion the evidence needs to support — a claim like "our method works well" is too vague to
  critique; "our method improves X metric by Y% over baseline Z" is checkable.
- In Task C, designing an ablation that changes more than one thing at once between conditions
  (e.g., "A only" also happens to use a different hyperparameter than "A and B") — this
  confounds the comparison and defeats the purpose of isolating each component's contribution.
- In Task D, concluding one method is "better" purely because its mean is higher, ignoring
  overlapping spread — this is exactly the single-run/insufficient-evidence mistake Week 13's
  lecture content warns against; the mini-challenge is designed to catch this.
- Treating "no reproducibility concerns found" as an acceptable Task B answer without genuinely
  checking all four categories (hyperparameters, seeds, compute, code/data) — most real papers
  have at least one.

**Debugging tip:** for Task D, compute and report the actual numeric mean/stdev for both
methods, not just a verbal impression — the exercise is explicitly about forming a judgment from
numbers, not intuition.

**Instructor tip:** use this lab's output to directly feed into students' approach for the
Paper Critique & Presentation assignment (assigned this week); explicitly tell students the
rubric structure mirrors this worksheet's four sections.

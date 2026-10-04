# Week 13 — Lecture Content: Research Methods for Statistical Learning Theory at the Postgraduate Level

## 1. How to Read a Cutting-Edge Statistical-Learning-Theory Paper
Identify, precisely: (a) **the claimed result** — is it an upper bound (an algorithm's analyzed
risk/regret), a lower bound (a minimax-style impossibility, as in Week 2), or both, with matching
rates (certifying optimality, exactly as Week 2's Gaussian-mean example did)?; (b) **the
assumptions it depends on** — distributional assumptions (sub-Gaussian? only finite variance, as
in Week 12?), independence/i.i.d.-ness, boundedness, a specific structural assumption (sparsity,
low rank); (c) **whether the proof or experiments actually support the claim at the stated
generality** — a bound proved under sub-Gaussian noise does not support a claim made for "general"
noise; an experiment on one small synthetic dataset does not support a claim of a method's
general practical superiority. This three-part check is the direct, graduate-to-postgraduate
escalation of the same careful-reading habit the graduate course's own research-methods week
introduced, now applied to this course's frontier material specifically.

## 2. The Problem-Statement Precision Test
A problem statement is the single most important sentence (or short paragraph) in a research
proposal, and it is the part students most often get wrong by writing a *topic* instead of a
*question*. Compare:
- **Vague (a topic, not a problem statement):** "I want to study robust statistics."
- **Precise and falsifiable:** "Does the median-of-means estimator's $O(\sigma\sqrt{\log(1/\delta)
  /n})$ error guarantee degrade to a provably worse rate under weakly dependent (mixing) rather
  than i.i.d. sampling, and if so, can a modified grouping scheme that respects the dependence
  structure recover the i.i.d. rate?"
The second version states exactly what would count as an answer (an error-rate result, positive
or negative), names the specific setting precisely enough to be attacked technically, and is not
already obviously settled by material already covered in this course (if the i.i.d. analysis of
Week 12 already answered it, it would not be a research question). The test: **could a
knowledgeable reader state what evidence would resolve the question, after reading only the
problem statement itself?** If not, it is not yet precise enough.

## 3. Related-Work Survey Standards
A postgraduate-level related-work survey of 5+ papers must do three things a weaker survey often
fails at:
1. **Accurately represent each paper's actual claim and finding** — not a vague paraphrase, and
   not what the student assumes the paper probably says based on its title or abstract alone.
2. **Relate the papers to each other**, not just to the student's own project — which papers
   build on which, where they agree, where they disagree or report conflicting findings, and what
   methodological choices (e.g., a different tail assumption, a different contamination model)
   explain any disagreement.
3. **Use the survey to motivate a specific, stated gap** — the survey should end by making clear
   exactly what question the existing literature, taken together, leaves open, and that gap should
   be the one the problem statement (§2) targets. Five isolated, accurate paper summaries with no
   synthesis at the end is not a survey in this sense, even if each individual summary is accurate.

## 4. Constructing a Feasibility Argument
When a full experiment cannot be completed within the semester, a rigorous **feasibility
argument** substitutes for preliminary results. A strong feasibility argument states, explicitly:
- **What must be true** for the proposed approach to work (the key assumption or mechanism the
  approach relies on — e.g., "this requires the dependence structure to be geometrically mixing,
  not just stationary").
- **The main technical risk** — the single most likely way the approach could fail, stated
  honestly rather than minimized (e.g., "the grouping scheme may not preserve independence closely
  enough for the Chernoff-boosting step of the median-of-means argument to go through unchanged").
- **Why the risk is judged manageable** — a reasoned argument (grounded in the related work, a
  smaller-scale sanity check, or a theoretical argument analogous to one derived elsewhere in this
  course, e.g., a Bernstein-style argument from Week 3) for why the risk, while real, does not
  make the approach obviously doomed. A feasibility argument that only lists what must be true,
  without naming and engaging the main risk honestly, is not yet rigorous — it is optimism, not
  argument.

## 5. How Postgraduate Evaluation Differs from the Graduate Standard
The graduate course's capstone is judged primarily on whether a literature review correctly
motivated a small experiment, and whether that experiment was executed and analyzed soundly — the
work is, in an important sense, already *done* by the time it is submitted. A postgraduate
research proposal is judged on something different: the soundness of a **plan** for research not
yet completed — exactly as a PhD qualifying exam or a thesis-proposal committee evaluates a
candidate's proposal before the dissertation work has happened. This means a postgraduate
proposal can be excellent even with no results at all (a rigorous feasibility argument substitutes
fully), and a proposal with a technically correct but trivial or already-settled problem statement
is weak regardless of how polished its writing is — the standard is squarely about the quality of
the *research plan*, not about polish or about having "something to show."

## 6. In-Class/Lab Exercise
See `lab-manuals/lab-13.md`: draft a one-paragraph problem statement for your own capstone
candidate topic (connected to one of this course's five pillars), subject it to the precision
test in §2 via structured peer critique, then annotate 5+ candidate related-work papers using the
three-part standard in §3.

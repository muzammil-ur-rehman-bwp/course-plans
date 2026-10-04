# Week 13 — Lecture Content: Research Methods at the Postgraduate Level

## 1. Writing a Precise, Falsifiable Problem Statement
A problem statement is the single most important sentence (or short paragraph) in a research
proposal, and it is the part students most often get wrong by writing a *topic* instead of a
*question*. Compare:
- **Vague (a topic, not a problem statement):** "I want to study multi-armed bandits."
- **Precise and falsifiable:** "Does adding a small, bounded amount of adversarial noise to a
  stochastic bandit's rewards degrade UCB1's regret from O(log T) to a provably worse rate, and
  if so, can a modified confidence-bound construction recover logarithmic regret under this
  noise model?"
The second version states exactly what would count as an answer (a regret-rate result, positive
or negative), names the specific setting precisely enough to be attacked technically, and is not
already obviously settled by material already covered in this course (if it were already
answered in Week 3's lecture, it would not be a research question). A useful test: **could a
knowledgeable reader state what evidence would resolve the question, after reading only the
problem statement itself?** If not, it is not yet precise enough.

## 2. Related-Work Survey Standards
A postgraduate-level related-work survey of 5+ papers must do three things a weaker survey
often fails at:
1. **Accurately represent each paper's actual claim and finding** — not a vague paraphrase, and
   not what the student assumes the paper probably says based on its title or abstract alone.
2. **Relate the papers to each other**, not just to the student's own project — which papers
   build on which, where they agree, where they disagree or report conflicting findings, and
   what methodological choices explain any disagreement.
3. **Use the survey to motivate a specific, stated gap** — the survey should end by making it
   clear exactly what question the existing literature, taken together, leaves open, and that gap
   should be the one the problem statement (§1) targets. A list of five isolated paper summaries
   with no synthesis at the end is not a survey in this sense, even if each individual summary is
   accurate.

## 3. Constructing a Feasibility Argument
When a full experiment cannot be completed within the semester, a rigorous **feasibility
argument** substitutes for preliminary results. A strong feasibility argument states, explicitly:
- **What must be true** for the proposed approach to work (the key assumption or mechanism the
  approach relies on).
- **The main technical risk** — the single most likely way the approach could fail, stated
  honestly rather than minimized.
- **Why the risk is judged manageable** — a reasoned argument (grounded in the related work, in
  a smaller-scale sanity check, or in a theoretical argument analogous to one derived elsewhere
  in this course, e.g., a regret-bound-style argument) for why the risk, while real, does not
  make the approach obviously doomed.
A feasibility argument that only lists what must be true, without naming and engaging the main
risk honestly, is not yet rigorous — it is optimism, not argument.

## 4. How Postgraduate Evaluation Differs from the Graduate Standard
The graduate course's capstone is judged primarily on whether a literature review correctly
motivated a small experiment, and whether that experiment was executed and analyzed soundly —
the work is, in an important sense, already *done* by the time it is submitted. A postgraduate
research proposal is judged on something different: the soundness of a **plan** for research not
yet completed — exactly as a PhD qualifying exam or a thesis-proposal committee evaluates a
candidate's proposal before the dissertation work has happened. This means a postgraduate
proposal can be excellent even with no results at all (a rigorous feasibility argument substitutes
fully), and a proposal with a technically correct but trivial or already-settled problem
statement is weak regardless of how polished its writing is — the standard is squarely about the
quality of the *research plan*, not about polish or about having "something to show."

## 5. In-Class/Lab Exercise
See `lab-manuals/lab-13.md`: draft a one-paragraph problem statement for your own capstone
candidate topic, subject it to the precision test in §1 via structured peer critique, then
annotate 5+ candidate related-work papers using the three-part standard in §2.

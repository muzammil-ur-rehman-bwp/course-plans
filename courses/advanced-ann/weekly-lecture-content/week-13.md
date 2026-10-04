# Week 13 — Lecture Content: Research Methods for Theoretical ML at the Postgraduate Level

## 1. Reading a Cutting-Edge Theory Paper Critically
A theoretical ML paper's central claim almost always depends on specific assumptions — frequently
an idealized limit (infinite width, as in Weeks 2–3; a specific data distribution; a restricted
architecture class). The postgraduate skill is reading precisely enough to answer, for any such
paper:
1. **What exactly is claimed?** State it as a precise proposition, not a vague paraphrase of the
   abstract.
2. **Under what assumptions does the claim hold?** Identify the idealized limit or restriction the
   proof or experiment actually depends on, and whether the paper is honest about that dependency.
3. **How strong is the supporting evidence?** For a theoretical claim: is it a full proof, a proof
   sketch with a stated gap, or an empirically-motivated conjecture? For an empirical claim: are
   the experiments controlled, with multiple seeds and reported spread, or a single run?
4. **What does the paper NOT establish**, that it might be informally read as establishing? (This
   course's own treatment of NTK, Week 2 §4, and grokking, Week 7 §5, model this skill directly —
   both sections state carefully what the covered result does and does not establish.)

## 2. The Precision Test for a Problem Statement
A problem statement is the single most important sentence (or short paragraph) in a research
proposal, and the part students most often get wrong by writing a *topic* instead of a *question*.
Compare:
- **Vague (a topic, not a problem statement):** "I want to study grokking."
- **Precise and falsifiable:** "Does the length of the grokking plateau, for a fixed modular-
  arithmetic task and architecture, scale predictably with weight-decay strength in a way a single
  fitted power-law relationship can capture, or does the relationship change qualitatively once
  weight decay crosses the threshold separating 'grokking eventually occurs' from 'it does not'?"
The second version states exactly what would count as an answer, names the setting precisely
enough to be attacked technically, and is not already settled by material this course has
covered. **The precision test:** could a knowledgeable reader state, after reading only the
problem statement itself, what evidence would resolve the question? If not, it is not yet precise
enough — and notably, it must also not already be answered by this course's own material (if
Week 7 already settled it, it is not a research question).

## 3. Related-Work Survey Standards
A postgraduate-level survey of 5+ papers must do three things a weaker survey often fails at:
1. **Accurately represent each paper's actual claim and finding** — not a paraphrase of its title
   or abstract, and not what the student assumes it probably says.
2. **Relate the papers to each other** — which build on which, where they agree or conflict, and
   what methodological differences (e.g., different architectures, different idealized limits)
   explain any disagreement.
3. **Use the survey to motivate a specific, stated gap** — ending by making explicit exactly what
   the existing literature, taken together, leaves open, and that gap should be the one the
   problem statement targets. Five accurate but isolated summaries, with no synthesis, is not a
   survey in the sense this capstone requires.

## 4. Constructing a Feasibility Argument
When a full experiment cannot be completed within the semester, a rigorous feasibility argument
substitutes for preliminary results, stating explicitly:
- **What must be true** for the proposed approach to work.
- **The main technical risk** — the single most likely way it could fail, named honestly, not
  minimized.
- **Why the risk is judged manageable** — reasoned argument grounded in the related work, a
  smaller-scale sanity check, or a theoretical argument analogous to one derived elsewhere in this
  course (e.g., a margin-style or scaling-law-style argument).
A feasibility argument that only lists reasons the approach will work, without naming and engaging
the main risk honestly, is optimism, not argument, and is graded accordingly (see
`assignments/capstone-rubric.md`).

## 5. How Postgraduate Evaluation Differs From Completed-Project Evaluation
A completed-project rubric judges whether a literature review correctly motivated an experiment
and whether that experiment was executed and analyzed soundly — the work is, in an important
sense, already done by the time it is submitted. A postgraduate research proposal is judged on the
soundness of a **plan** for research not yet completed, exactly as a thesis-proposal committee
evaluates a candidate's proposal. A proposal can be excellent with no results at all (a rigorous
feasibility argument substitutes fully); a proposal with a technically correct but already-settled
or trivial problem statement is weak regardless of writing polish.

## 6. In-Class/Lab Exercise
See `lab-manuals/lab-13.md`: critique the provided theory-paper excerpt using §1's four questions;
draft a one-paragraph problem statement for your own capstone candidate topic and subject it to
the §2 precision test via structured peer critique; annotate 5+ candidate related-work papers using
the §3 standard.

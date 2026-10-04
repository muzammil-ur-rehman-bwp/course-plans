# Research Capstone — Proposal Guidelines

**Problem-statement check-in:** Week 8 | **Draft proposal due:** Week 15 | **Final proposal +
oral defense:** Week 16 | **Weight:** Research Proposal Capstone, 40% of course grade total

## What This Capstone Is
This capstone is a **research proposal**, not a completed project — a PhD-qualifying-exam-style
deliverable evaluated the way a thesis-proposal committee evaluates a candidate's proposal before
the dissertation work has happened. It can score excellently with no experimental results at all,
provided the problem statement, survey, proposed approach, and feasibility reasoning are sound;
conversely, a technically correct but trivial or already-settled problem statement is weak
regardless of how polished the writing or how much code was run. This is a deliberate departure
from the graduate course's capstone (a literature review plus a reproduced or extended
experiment, judged on execution) — here the object being judged is the soundness of a **plan**
for research not yet completed.

## Required Components
Every submission must contain all four of the following (see `course-plan.md` §5, Week 13 and
`course-contents.md`'s Research Proposal Capstone section for the full framing):
1. **Problem statement.** A precise, falsifiable question or gap — not a restatement of a topic.
   Apply the Week 13 precision test: could a knowledgeable reader state, after reading only this
   statement, what evidence would resolve it? "I want to study Mixture-of-Experts" is a topic, not
   a problem statement; "does replacing the standard coefficient-of-variation auxiliary loss with
   a hard capacity-constraint-based routing rule reduce router collapse when $N\gg k$, without the
   accuracy cost a hard capacity constraint is generally assumed to impose?" is a problem
   statement.
2. **Related-work survey (5+ papers).** Must accurately represent what each paper claims and
   found (not a paraphrase of its title or abstract), relate the papers to each other (who builds
   on whom, where they agree or conflict), and end by using the survey to motivate the specific
   gap named in the problem statement. Five isolated, accurate summaries with no synthesis is not
   a survey in the sense this capstone requires.
3. **Proposed novel approach or extension.** Your own formulation — it may combine or extend
   existing ideas from the survey, but must be clearly distinguishable from simply restating one
   surveyed paper's contribution as if it were your own.
4. **Preliminary results or a feasibility argument.** Either a small implemented pilot
   demonstrating the approach is workable, or — when a full pilot is not feasible within the
   semester — a rigorous feasibility argument stating explicitly: what must be true for the
   approach to work, the single most likely way it could fail (the main technical risk, named
   honestly, not minimized), and a reasoned case for why that risk does not make the approach
   obviously doomed.

## Topic Selection
Each student formulates an original research question connected to one of this course's topics
(SDE-based score generative modeling and advanced diffusion techniques; Mixture-of-Experts and
sparse architectures; in-context learning mechanics; preference-based alignment engineering;
efficient inference; neural architecture search; systems-view scaling and large-scale training
engineering; frontier generative-modeling sampling efficiency), or to adjacent territory with
instructor approval. Example topics (for inspiration, not a closed list, drawn from
`course-contents.md`): a principled load-balancing mechanism for MoE routing; a mechanistic test
of how far the induction-heads/implicit-gradient-descent picture of in-context learning
generalizes; a DPO variant robust to a specific, well-defined class of Bradley-Terry violations; a
jointly-trained draft/target speculative-decoding scheme and its effect on acceptance rate; a
differentiable-NAS search-space extension targeting a specific efficiency-accuracy tradeoff; a
compute-optimal-allocation refinement for a specific architecture family (e.g., MoE models, where
the standard $C\approx 6ND$ approximation may need adjustment). Topics requiring deep theoretical
NN-theory, alignment-as-research-agenda, or statistical-learning-theory content owned by the
sibling postgraduate courses (ANN, AI, ML) require instructor pre-approval and must still center
on this course's own techniques.

## Milestones
- **Week 8 — Problem-statement check-in.** An informal check-in with the instructor on a
  tentative, specific (even if not yet fully falsifiable) problem statement.
- **Week 13 — Research-methods workshop.** Problem statement refined to pass the precision test
  (via structured peer critique) and 3–5 candidate related-work papers annotated (see
  `lab-manuals/lab-13.md`).
- **Week 15 — Draft proposal due.** A complete written draft (all four required components) is
  submitted and subjected to a structured, thesis-committee-style peer-review workshop (see
  `lab-manuals/lab-15.md`); a written revision plan is produced in response.
- **Week 16 — Final proposal submission and oral defense.** The final written proposal is
  submitted and defended orally in a qualifying-exam/thesis-proposal-defense format: problem
  statement, related work, proposed approach, feasibility argument or preliminary results,
  anticipated risks, and committee-style Q&A (see `presentations/capstone-presentation-template.md`).

## Individual Work and Academic Integrity
The capstone is individual work (see `course-plan.md` §10). Any collaboration — e.g., discussing
a problem statement with a peer during the Week 13 or Week 15 workshops — must be disclosed in
the proposal's acknowledgments. A related-work survey must accurately represent what each cited
paper actually claims and found; a proposed approach must be your own formulation; a feasibility
argument or preliminary result must be your own reasoning or your own experimentation. Uncredited
reuse of another author's ideas, text, code, or results — including presenting a paper's reported
results as your own preliminary findings — is treated with the same seriousness as plagiarism in
a thesis proposal.

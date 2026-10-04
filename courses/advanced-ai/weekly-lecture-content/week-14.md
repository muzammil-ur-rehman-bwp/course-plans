# Week 14 — Lecture Content: Current Frontier Topics Survey

**This week is explicitly flagged: it covers a fast-moving research area, and the specific
techniques, benchmarks, and claims discussed below (and in the instructor's chosen current
readings) may date quickly. The graded skill is locating, reading, and critically situating
current primary sources against this course's foundations — not memorizing any particular
current result as a fixed fact.**

## 1. How to Read This Week's Content
Unlike Weeks 2–12, which cover settled, citable results (regret bounds, PPAD-completeness, the
VCG truthfulness proof), this week surveys **active, unsettled** research directions. The
instructor selects 2–3 specific current directions each time the course is offered, chosen from
candidates such as the three sketched below; what follows is a template for *how* to engage with
whatever specific papers are assigned, not a fixed, permanent reading list.

## 2. Candidate Direction 1 — Scalable Oversight, Maturing Beyond Week 9
Week 9 introduced debate and amplification at the level of their general mechanism. Current
work in this area typically asks sharper, more specific questions: under what conditions does a
debate-style protocol actually make dishonesty harder to sustain than honesty, for a judge with
specific, bounded capabilities? What happens when both debaters have an incentive to collude
rather than genuinely oppose each other? **Builds on:** Week 9 (scalable oversight), Week 6
(game-theoretic framing of strategic interaction between the debating systems). **Open
question (illustrative, not a settled citation):** whether debate's theoretical guarantees (where
they exist at all) survive contact with judges who have realistic, bounded attention and
expertise, rather than idealized unbounded judges.

## 3. Candidate Direction 2 — Mechanistic Interpretability, Extending Week 10
Week 10 introduced mechanistic interpretability as the project of identifying circuits/features
rather than post-hoc attributions. Current work in this area typically asks: can specific,
individually-identified circuits be composed into an account of a model's behavior on a broader
task, or does circuit-level understanding fail to scale past small, cherry-picked examples?
**Builds on:** Week 10 (mechanistic interpretability, the post-hoc/mechanistic gap). **Open
question (illustrative):** whether current circuit-discovery techniques can be made systematic
and automated enough to characterize a meaningful fraction of a large model's behavior, rather
than remaining a labor-intensive, case-by-case research craft.

## 4. Candidate Direction 3 — Multi-Agent Training Approaches, Extending Week 5
Week 5 introduced self-play's general mechanism (an automatically scaling curriculum from
training against an improving version of oneself). Current work in this area typically asks:
how should a population of diverse past checkpoints (rather than only the single most recent
version) be sampled as opponents to avoid cyclic, non-generalizing behavior, and how does this
interact with the non-stationarity problem from Week 5's independent-learner analysis? **Builds
on:** Week 5 (multi-agent RL, non-stationarity, self-play). **Open question (illustrative):**
whether training against a diverse opponent population reliably produces strategies that
generalize to genuinely novel opponents, or only to opponents drawn from a similar training
distribution.

## 5. The Transferable Skill: Problem-Statement Extraction
For whichever current paper(s) the instructor assigns this week, practice the Week 13 skill
directly: state the paper's actual problem statement in your own words (not its title), identify
which of this course's pillars it builds on, and identify one specific question the paper leaves
open — this is direct, deliberate rehearsal for identifying your own capstone's problem statement
and gap.

## 6. In-Class/Lab Exercise
See `lab-manuals/lab-14.md`: for one instructor-assigned current paper, write a structured brief
containing (a) its problem statement in your own words, (b) which course pillar(s) it builds on
and how, and (c) one specific open question it leaves unresolved, explicitly distinguishing
claims the paper supports with evidence from claims it merely speculates about.

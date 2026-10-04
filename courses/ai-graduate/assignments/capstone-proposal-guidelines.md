# Research Capstone — Proposal Guidelines

**Topic selection opens:** Week 7–8 (informal discussion with instructor) | **Formal proposal
due:** End of Week 8 | **Weight:** part of the Research Capstone (25% of course grade total)

## What to Submit
A 1–2 page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Subtopic:** which AI subtopic from this course (or closely adjacent, with instructor
   approval) the project addresses — e.g., advanced search, game theory/multi-agent systems,
   automated reasoning, rigorous planning, MDPs/RL, POMDPs, or probabilistic graphical models.
2. **Motivating question:** what specific question or gap is the project investigating? (This
   need not be fully refined yet — Week 13–15 will sharpen it — but it must be more specific
   than "I want to study X.")
3. **Planned literature:** 3–5 candidate papers (title/authors/venue/year) the student plans to
   review; these may change as the literature review develops, but the proposal must show a
   genuine starting point, not a placeholder.
4. **Planned experiment:** a reproduced or extended experiment from one of the candidate papers,
   or a focused comparison/extension using techniques from this course — stated concretely
   enough that a feasible scope is clear (e.g., "reproduce the convergence-rate comparison from
   [paper] for value iteration under 3 discount factors on a grid-world of my own design" is
   concrete; "study reinforcement learning" is not).
5. **Team:** individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
build on techniques covered in this course; topics requiring deep neural-network, classical-ML,
deep-learning, or deep-KR-formalism content (owned by the sibling graduate courses) require
instructor pre-approval and must still center on this course's own techniques.

## Example Topics (for inspiration, not a closed list)
Comparing IDA*/SMA*/A* memory-time tradeoffs empirically on a chosen domain; a small multi-agent
Q-learning experiment examined through the lens of Nash equilibria; extending a DPLL
implementation with a simple clause-learning-inspired heuristic and measuring the speedup; an
empirical study of planning-graph heuristic quality across several STRIPS domains; a comparison
of value/policy iteration convergence rates under different discount factors or reward
structures; an empirical study of rejection sampling vs. likelihood weighting efficiency as
evidence rarity varies; a focused reproduction of a specific result from a current AAAI/IJCAI/
NeurIPS-style paper on multi-agent RL, XAI, or AI safety/alignment.

# Week 15 — Lecture Content: Research Proposal Work Session

## 1. What This Week Is For
No new technical material is introduced this week. The session is structured work time: every
student should arrive with a problem statement (Week 13), a substantially complete 5+ paper
related-work survey (Weeks 13–14), a stated proposed novel approach, and at least a skeleton
feasibility argument or a preliminary pilot result. The work of this week is to turn that
material into a complete, defensible draft proposal and to subject it to critical peer feedback
before the Week 16 defense.

## 2. The Peer-Feedback Protocol
Feedback this week is modeled explicitly on how a thesis-proposal committee questions a
candidate — critical, not merely encouraging. For each proposal draft reviewed, a peer reviewer
should answer four questions in writing, each with a specific justification (not just a rating):
1. **Is the problem statement precise and genuinely open?** (Apply the Week 13 precision test:
   could a knowledgeable reader state what evidence would resolve it? Is it already settled by
   material covered in this course or in the student's own cited survey?)
2. **Does the related-work survey correctly represent the cited papers, and does it motivate the
   stated gap?** (Spot-check: does the student's summary of at least one cited paper match what
   that paper, read directly, actually claims?)
3. **Is the proposed approach actually novel relative to the surveyed work, or a restatement of
   it?** (A proposal that reads as "I will do exactly what paper X already did" has not yet
   identified a genuine contribution.)
4. **Is the feasibility argument honest about the main risk?** (A feasibility argument that lists
   only reasons the approach will work, with no stated risk at all, has not met the Week 13
   standard.)

## 3. A Worked Example of Weak vs. Strong Feedback
**Weak feedback:** "Looks good, I like the topic." (Not actionable; does not engage any of the
four questions above.)
**Strong feedback:** "Your problem statement asks whether a modified UCB confidence bound
recovers logarithmic regret under bounded adversarial noise — that's precise and testable. But
your related-work survey summarizes Auer et al. (2002) and a robust-bandits paper separately
without ever stating how they relate — does the robust-bandits paper already answer your
question for a similar noise model? If so, your gap needs to be narrower and more specific than
currently stated; if not, say explicitly why their noise model differs from yours." This is the
standard of feedback expected this week.

## 4. Revising in Response to Feedback
After receiving peer feedback, each student should produce a short, explicit revision plan: for
each of the four questions above where feedback identified a weakness, state the specific change
that will be made before the Week 15 draft deadline. A revision plan that restates the original
draft's content without addressing the specific critique received is not sufficient.

## 5. Checklist Before Submitting the Week 15 Draft
- [ ] Problem statement passes the Week 13 precision test.
- [ ] Related-work survey covers 5+ papers, accurately represented, related to each other, and
  ends with an explicitly stated gap.
- [ ] Proposed approach is clearly distinguishable from any single surveyed paper's contribution.
- [ ] Feasibility argument (or preliminary result) names the main risk honestly, not just reasons
  for optimism.
- [ ] At least one round of peer feedback has been received and a revision plan written.

## 6. In-Class/Lab Exercise
See `lab-manuals/lab-15.md`: the structured peer-review workshop implementing the protocol in
§2, applied to each student's current draft, producing the revision plan required by §4.

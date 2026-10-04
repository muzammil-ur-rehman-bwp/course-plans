# Week 10 Summary — Multi-Agent Belief Merging

**Key takeaways:**
- Belief merging generalizes Dalal revision to a profile of peer belief sets: the merged result's
  models minimize aggregate (sum/max) distance to the profile, subject to integrity constraints.
- The merging postulates (IC0–IC3, generalizing AGM) require respecting integrity constraints,
  conjunctive behavior when jointly consistent, commutativity across agents, and
  equivalence-invariance.
- Merging (peer belief sets, none privileged) is structurally different from AGM revision (one
  privileged belief set, one new trusted input).

**You should now be able to:** compute a distance-based merge by hand and in code; check merging
postulates on a worked example; state the merging/revision structural distinction precisely.

**Next week:** Explanation and justification research — proof-tree/justification explanation for
expressive-DL entailments. **Quiz 4** (Weeks 7–9) and **Assignment 2 assigned** this week.

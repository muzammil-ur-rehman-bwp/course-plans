# Week 12 Summary — Loss-Landscape Geometry and the Lottery Ticket Hypothesis Revisited

**Key takeaways:**
- Linear interpolation between independently trained minima typically shows a loss barrier; a
  simple nonlinear (bend-point) path can connect the same minima while keeping loss low — mode
  connectivity — without implying the minima are identical or the landscape is flat.
- The linear-mode-connectivity refinement sharpens the Lottery Ticket Hypothesis: winning tickets
  retrained from their original initialization stay linearly connected across independent reruns,
  unlike randomly reinitialized masks.
- Current critiques flag scale/learning-rate sensitivity in finding tickets reliably, and an open
  question about whether pruning evidence is best read as "already there at initialization" or an
  artifact of the iterative procedure itself.

**You should now be able to:** implement and interpret linear vs. nonlinear interpolation between
minima; state the linear-mode-connectivity refinement of LTH precisely; articulate the open
question about what pruning evidence actually establishes.

**Next week:** Research methods for theoretical ML at the postgraduate level — reading cutting-
edge theory papers, identifying genuinely open problems, and structured capstone work time.

# Week 2 Summary — Advanced Search

**Key takeaways:**
- A*'s practical bottleneck is usually memory (O(b^d)), not time.
- IDA* achieves O(d) space by running repeated depth-first contours bounded by an increasing
  f-cost threshold; it preserves A*'s optimality and completeness.
- Bidirectional search can give an exponential speedup (O(b^(d/2)) vs. O(b^d)) when the
  transition model supports searching backward from the goal.
- SMA* behaves like A* under a fixed memory budget, discarding and backing up the worst frontier
  leaves, and degrades gracefully instead of running out of memory.

**You should now be able to:** implement IDA*; derive and state the space complexity of IDA*,
bidirectional search, and SMA* relative to plain A*; explain when each is the right tool.

**Next week:** Game theory and adversarial search I — a formal correctness argument for
alpha-beta pruning, and an introduction to normal-form games and Nash equilibrium.

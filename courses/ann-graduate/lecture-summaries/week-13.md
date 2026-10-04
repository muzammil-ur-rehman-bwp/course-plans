# Week 13 Summary — The Lottery Ticket Hypothesis and Pruning

**Key takeaways:**
- Frankle and Carbin's Lottery Ticket Hypothesis: a dense network at initialization contains a
  sparse "winning ticket" subnetwork that, trained from that same initialization, matches the full
  network's accuracy.
- Iterative magnitude pruning (train, prune smallest-magnitude weights, reset survivors to
  $\theta_0$, retrain, repeat) is the standard procedure for finding one.
- The random-reinitialization control (same mask, fresh random weights) training worse than the
  winning ticket is the key evidence that initialization — not just architecture — matters.

**You should now be able to:** implement iterative magnitude pruning and the random-
reinitialization control, and explain why that control is essential to the hypothesis's claim.

**Next week:** information-theoretic perspectives — the information bottleneck idea, presented as
a debated, active research area.

# Lab Manual 7 — The VCG Mechanism and Its Truthfulness

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Implement the VCG mechanism for a single-item auction, confirm it reduces exactly to the
second-price rule, and empirically verify that truthful reporting maximizes a bidder's utility.

## Setup
1. Reuse your course virtual environment.
2. Create `lab07.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `vcg_mechanism(outcomes, valuations)` exactly as in
   the Week 7 lecture content.
2. **Task B — Reduction check:** instantiate a single-item auction (one outcome per bidder,
   "agent i wins") for 4 bidders with provided true valuations. Confirm the chosen outcome is
   the highest-value bidder and the winner's payment equals the second-highest valuation, for at
   least 3 different randomly generated valuation profiles.
3. **Task C — Truthfulness sweep:** fix the other 3 bidders' true reports. Sweep bidder 1's
   *reported* value across a range spanning below and above their true valuation, compute their
   resulting utility (true value minus payment, or 0 if they lose) at each reported value, and
   plot utility vs. reported value. Confirm the utility is maximized exactly at the truthful
   report.
4. **Task D — Mini-challenge:** extend `vcg_mechanism` to a small 2-item combinatorial
   allocation problem with 3 bidders who have additive valuations over bundles (valuations
   provided as a dict per bidder per bundle). Verify the allocation rule still selects the
   welfare-maximizing bundle assignment, and comment, in 2–3 sentences, on how computing
   `argmax_o` becomes more expensive as the number of items grows.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the Task C truthfulness plot.

## Submission
Export/submit `lab07.ipynb` via the course submission system by the end of the lab session.

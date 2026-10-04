# Lab Manual 4 — Multi-Agent Auction Simulation

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Simulate competing bidding agents in first-price and second-price (Vickrey) auctions and verify
the truthfulness property of second-price auctions empirically.

## Setup
1. Reuse your course virtual environment.
2. Create `lab04.ipynb`.

## Procedure
1. **Task A — Second-price auction:** implement `run_second_price_auction` from the Week 4
   lecture content; run it on 5 randomly generated bidder-valuation sets and report the winner,
   price, and surplus each time.
2. **Task B — First-price auction with shading:** implement a first-price auction where each
   bidder shades their bid by a fixed fraction (e.g., bids 80% of true value); run on the same
   valuation sets and compare winner/price/surplus to Task A.
3. **Task C — Truthfulness check:** for one fixed set of other bidders' valuations, sweep one
   bidder's *bid* (not valuation) in a second-price auction across values both above and below
   their true valuation, and plot/report their surplus at each bid — confirming surplus is
   maximized at bid = true valuation.
4. **Task D — Mini-challenge:** classify three given multi-agent scenarios (provided by the
   instructor) as cooperative/competitive and zero-sum/general-sum, with a one-sentence
   justification each.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab04.ipynb` via the course submission system by the end of the lab session.

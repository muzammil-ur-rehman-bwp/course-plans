# Lab Notes 4 — Multi-Agent Auction Simulation

**Concept recap:** in a second-price (Vickrey) auction, bidding one's true valuation is a
dominant strategy; in a first-price auction, the optimal bid depends on beliefs about other
bidders and is generally shaded below true valuation.

**Common pitfalls:**
- Computing the second-price auction's payment as the *winner's own bid* instead of the
  *second-highest bid* — this is the single defining feature of a Vickrey auction, and getting
  it wrong silently turns Task A into a first-price auction.
- In Task C, sweeping the bid without holding the *other* bidders' valuations/bids fixed — the
  truthfulness property is about one bidder's best response holding everyone else fixed, exactly
  mirroring the Nash equilibrium definition from Week 3.
- Confusing "shading a bid" (Task B) with "lying about valuation" conceptually — shading is a
  rational strategic response to the first-price mechanism's rules, not a mistake; the lab asks
  students to observe its effect on outcomes, not to judge it as wrong.
- In Task D, defaulting to "it's complicated" without committing to a classification — the
  exercise wants a definite classification with justification, even for ambiguous-seeming cases.

**Debugging tip:** for Task C, if the surplus curve does not peak exactly at bid = true
valuation, check whether ties are being broken consistently (e.g., does bidding exactly at the
second-highest value count as winning or losing in your implementation?).

**Instructor tip:** have students predict, before running Task C, what shape they expect the
surplus-vs-bid curve to take (flat above true value then dropping to zero below it, or something
else) — this tests whether they actually internalized the Week 4 truthfulness argument's
case-by-case structure, not just memorized the conclusion.

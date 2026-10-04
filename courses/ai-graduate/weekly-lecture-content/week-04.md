# Week 4 — Lecture Content: Game Theory II & Multi-Agent Systems

## 1. Cooperative vs. Competitive Multi-Agent Systems
A **multi-agent system (MAS)** has multiple agents whose joint actions determine outcomes. Two
useful axes of classification:
- **Cooperative vs. competitive:** in a cooperative MAS agents share a common goal (e.g.,
  multiple warehouse robots jointly minimizing total delivery time); in a competitive MAS agents
  have individually conflicting goals (e.g., bidders in an auction).
- **Zero-sum vs. general-sum:** in a zero-sum game, one player's gain is exactly another's loss
  (ΣᵢUᵢ = constant); in a general-sum game, payoffs are not forced to sum to a constant, so
  mutually beneficial (or mutually harmful) outcomes are possible — most multi-agent systems in
  practice are general-sum.

**Coordination problems (brief).** Even purely cooperative agents can fail to coordinate without
communication — e.g., two equally good joint strategies exist, but each agent might pick a
different one independently, yielding a mismatched outcome strictly worse than either coordinated
option. This course only notes the existence of this problem; a full treatment of communication
protocols and coordination mechanisms is beyond this course's scope.

## 2. Mechanism Design: Designing the Rules of the Game
In classical game theory, the game's rules are given and we solve for equilibrium behavior.
**Mechanism design** inverts this: we are given a *desired* outcome (e.g., allocate a good to
whoever values it most) and must design the rules (the mechanism) so that self-interested agents'
equilibrium behavior achieves that outcome. This is a genuinely important AI application area —
it underlies ad auctions, resource allocation in multi-agent systems, and matching markets.

## 3. Auction Theory, Conceptually
Consider a single item and n bidders, where bidder i privately knows their own valuation vᵢ (how
much the item is worth to them) but not the others' valuations.

**First-price sealed-bid auction.** Every bidder submits a bid; the highest bidder wins and pays
their own bid. Here a rational bidder should **not** bid their true value vᵢ — bidding vᵢ would
win but yield zero surplus (payoff = vᵢ − vᵢ = 0), so bidders shade their bids below their true
valuation, and figuring out the optimal shading requires reasoning about the other bidders'
strategies.

**Second-price sealed-bid (Vickrey) auction.** Every bidder submits a bid; the highest bidder
wins but pays the *second-highest* bid, not their own. This seemingly small change has a famous
consequence:

**Claim (truthfulness of the Vickrey auction).** Bidding one's true valuation (bᵢ = vᵢ) is a
dominant strategy — it is optimal for bidder i regardless of what the other bidders bid.

**Argument (sketch).** Let b₋ᵢ be the highest bid among the other bidders. If bidder i bids
bᵢ > b₋ᵢ, they win and pay b₋ᵢ, for a surplus of vᵢ − b₋ᵢ. Compare three cases against bidding
truthfully (bᵢ = vᵢ):
- If vᵢ > b₋ᵢ: bidding truthfully also wins, with the same surplus vᵢ − b₋ᵢ (the price paid
  depends on b₋ᵢ, not on bᵢ, as long as bᵢ stays above b₋ᵢ). Bidding higher than vᵢ cannot help
  (it still pays b₋ᵢ and wins the same way); bidding lower, down to b₋ᵢ, also still wins with the
  same surplus — but bidding *below* b₋ᵢ loses the item and surplus drops to 0, which is worse
  whenever vᵢ > b₋ᵢ (a positive surplus was available).
- If vᵢ < b₋ᵢ: bidding truthfully loses (surplus 0). Bidding higher than vᵢ to try to win would
  mean paying b₋ᵢ > vᵢ if it wins, for a *negative* surplus — strictly worse than losing with 0.
- If vᵢ = b₋ᵢ: the outcome is a tie with surplus 0 either way.

In every case, deviating from bᵢ = vᵢ never strictly helps and can strictly hurt, so truthful
bidding is a dominant strategy. This is the key reason second-price/Vickrey-style mechanisms (and
their generalizations, e.g., the Vickrey-Clarke-Groves mechanism) are widely used in real
resource-allocation and ad-auction systems: they make truthful behavior rational, which simplifies
both bidder strategy and system design.

```python
import random

def run_second_price_auction(valuations):
    """valuations: dict bidder_id -> true value. Truthful bidding assumed (dominant strategy)."""
    bids = dict(valuations)  # each bidder bids its true value
    winner = max(bids, key=bids.get)
    sorted_bids = sorted(bids.values(), reverse=True)
    price = sorted_bids[1] if len(sorted_bids) > 1 else 0
    surplus = valuations[winner] - price
    return winner, price, surplus

if __name__ == "__main__":
    random.seed(0)
    valuations = {f"bidder_{i}": random.randint(10, 100) for i in range(5)}
    winner, price, surplus = run_second_price_auction(valuations)
    print(f"Winner: {winner}, pays: {price}, surplus: {surplus}")
```

## 4. Where This Connects Later
Week 14 revisits multi-agent systems through the lens of multi-agent reinforcement learning: once
agents *learn* their strategies from experience rather than being handed an equilibrium, the
underlying game-theoretic structure from this week (cooperative/competitive, zero-sum/general-sum,
equilibrium concepts) becomes the lens for understanding what the learning agents are converging
toward (or failing to converge toward).

## 5. In-Class/Lab Exercise
Extend `run_second_price_auction` to also simulate a first-price auction where bidders shade
their bids by a fixed fraction of their valuation; compare the winner, price, and surplus across
several random valuation draws, and discuss why the first-price result depends on the (arbitrary)
shading strategy while the second-price result does not.

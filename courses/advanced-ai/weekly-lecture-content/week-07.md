# Week 7 — Lecture Content: Algorithmic Game Theory II — Mechanism Design

## 1. The Mechanism Design Problem
Mechanism design inverts game theory's usual question. Game theory asks: given a game, what will
self-interested players do? Mechanism design asks: given a desired social outcome, what game
(what allocation rule and payment rule) should we design so that self-interested players'
equilibrium behavior *produces* that outcome? The central difficulty is that each player's true
valuation for each possible outcome is private information — the mechanism only observes
*reported* valuations, which a self-interested player will misreport if doing so is profitable.

## 2. The Second-Price (Vickrey) Auction, Revisited
Single item, n bidders, bidder i has true private value vᵢ. The second-price auction: award the
item to the highest bidder, who pays the *second*-highest bid. Claim: truthful bidding
(bidding bᵢ = vᵢ) is a dominant strategy. Proof sketch: if bidder i wins by bidding bᵢ > their
true value vᵢ and the second-highest bid lies between vᵢ and bᵢ, they win but pay more than their
value — a loss they could have avoided (by losing instead) had they bid truthfully; if they bid
bᵢ < vᵢ and the highest other bid lies between bᵢ and vᵢ, they lose a sale they would have
profited from had they bid truthfully. In every other case, bidding truthfully vs. any other
value yields the identical outcome (same winner, same price), because bᵢ only affects *whether*
i wins (by comparison to the others' fixed bids), never the price i pays if they win (which is
set by the second-highest bid, independent of how high i's own winning bid was). So truthful
bidding never does worse, and sometimes does strictly better, than any misreport — a dominant
strategy.

## 3. The General VCG Mechanism
Generalizing: n agents, a set of feasible outcomes O, each agent i has a private valuation
function vᵢ(o) for each outcome o. Agents report (possibly untruthful) valuations v̂ᵢ. The VCG
mechanism:
1. **Allocation rule:** choose o* = argmax_o Σᵢ v̂ᵢ(o) — the outcome maximizing reported total
   welfare.
2. **Payment rule:** agent i pays
   ```
   pᵢ = [ max_o Σ_{j≠i} v̂ⱼ(o) ]  −  [ Σ_{j≠i} v̂ⱼ(o*) ]
      =  (others' best achievable welfare without i)  −  (others' welfare at the chosen outcome)
   ```
   i.e., agent i pays exactly the **externality** it imposes on everyone else by being present
   and taking part in the chosen outcome, rather than the outcome that would have maximized
   everyone else's welfare had agent i not participated.

```python
import itertools

def vcg_mechanism(outcomes, valuations):
    """outcomes: list of outcome labels. valuations: dict agent -> dict outcome -> reported value.
    Returns (chosen outcome, dict of payments)."""
    agents = list(valuations.keys())

    def welfare(subset_agents, outcome):
        return sum(valuations[a][outcome] for a in subset_agents)

    o_star = max(outcomes, key=lambda o: welfare(agents, o))
    payments = {}
    for i in agents:
        others = [a for a in agents if a != i]
        best_without_i = max(welfare(others, o) for o in outcomes)
        others_at_chosen = welfare(others, o_star)
        payments[i] = best_without_i - others_at_chosen
    return o_star, payments
```

## 4. Proving VCG's Truthfulness
Fix agent i; suppose everyone else reports truthfully. If i reports v̂ᵢ, the mechanism picks
o(v̂ᵢ) = argmax_o [ v̂ᵢ(o) + Σ_{j≠i} vⱼ(o) ]. Agent i's **true** utility (value minus payment) is:
```
uᵢ(v̂ᵢ) = vᵢ(o(v̂ᵢ)) − pᵢ(v̂ᵢ)
        = vᵢ(o(v̂ᵢ)) − [ max_o Σ_{j≠i} vⱼ(o) − Σ_{j≠i} vⱼ(o(v̂ᵢ)) ]
        = [ vᵢ(o(v̂ᵢ)) + Σ_{j≠i} vⱼ(o(v̂ᵢ)) ]  −  max_o Σ_{j≠i} vⱼ(o)
```
The second term does not depend on v̂ᵢ at all (it is a fixed quantity determined entirely by the
other agents' true valuations). So maximizing uᵢ(v̂ᵢ) over the report v̂ᵢ is equivalent to
maximizing the first bracketed term, vᵢ(o(v̂ᵢ)) + Σ_{j≠i} vⱼ(o(v̂ᵢ)), over whichever outcome the
report induces the mechanism to choose. But by definition, o(v̂ᵢ) is chosen by the mechanism to
maximize v̂ᵢ(o) + Σ_{j≠i} vⱼ(o) — **if agent i reports truthfully (v̂ᵢ = vᵢ), the mechanism
directly maximizes exactly the quantity that determines i's own utility**, over all outcomes in
O, which is the best any report could possibly induce (since no other report can make the
mechanism choose an outcome outside O, and truthful reporting already achieves the best outcome
in O for this exact objective). Hence truthful reporting maximizes agent i's utility no matter
what the other agents report — a **dominant strategy**. This is the standard Groves-mechanism
sufficiency argument, of which VCG's welfare-maximizing allocation rule is the canonical instance,
and the second-price auction (§2) is its single-item specialization.

## 5. Applications: Ad Auctions and Resource Allocation
**Sponsored-search (ad) auctions** sell advertising slots to bidders who value clicks
differently; the generalized second-price auction widely used in practice is explicitly *not*
exactly VCG (it charges the next-highest bid per click-weighted slot rather than the true VCG
externality), which is a deliberate practical trade-off — it is simpler to explain and run, at
the cost of truthfulness no longer being a dominant strategy in general with multiple slots.
**Resource allocation** (e.g., allocating computational resources, spectrum, or transportation
capacity among competing agents) uses VCG-style mechanisms when exact truthfulness and
efficiency are paramount, but real deployments often depart from exact VCG because of payment
budget constraints, revenue requirements, or computational cost of solving the exact
welfare-maximization problem for combinatorial outcome spaces (which can itself be NP-hard,
independent of the mechanism-design question).

## 6. In-Class/Lab Exercise
Implement `vcg_mechanism` for a single-item auction (outcomes = "agent i wins," one per bidder)
with 4 bidders' true valuations, and confirm it reduces exactly to the second-price auction rule.
Then sweep one bidder's *reported* value above and below their true value (holding the mechanism
and others' reports fixed) and plot their resulting utility, confirming it is maximized exactly
at the truthful report.

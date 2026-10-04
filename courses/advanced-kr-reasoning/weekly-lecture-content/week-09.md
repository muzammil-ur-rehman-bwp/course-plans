# Week 9 — Lecture Content: Formal Verification for Knowledge-Based Systems

(Delivered after the midterm exam.)

## 1. The Model-Checking Problem
A finite **Kripke structure** M = ⟨S, S₀, R, L⟩ consists of a finite set of states S, a set of
initial states S₀ ⊆ S, a **total** transition relation R ⊆ S×S (every state has at least one
successor, so every path is infinite), and a labeling function L: S → 2^AP assigning each state
the set of atomic propositions true there. Given M and a temporal-logic formula φ (here, LTL —
recall the graduate course's syntax: X, G, F, U, and Boolean connectives, interpreted over an
infinite execution path), the **model-checking problem** is to decide:
```
M ⊨ φ   iff   every infinite path through M starting from some state in S₀ satisfies φ
```

## 2. The Automata-Theoretic Approach
The standard decision procedure: translate **¬φ** into a **Büchi automaton** A_¬φ — a finite
automaton over infinite words with an *acceptance condition* (a designated set of accepting
states that must be visited **infinitely often** along an accepted run) — constructed so that
A_¬φ accepts exactly the infinite words (traces) that **violate** φ. Form the **product**
M ⊗ A_¬φ (a combined automaton tracking M's current state and A_¬φ's current state together, with
a transition whenever both components can move together consistently). Then:
```
M ⊨ φ   iff   the product M ⊗ A_¬φ has NO reachable accepting cycle
              (no cycle, reachable from an initial state, that visits an accepting state
               infinitely often along some run through the cycle)
```
Intuitively: if some execution of M could violate φ, that execution corresponds to a path through
M that A_¬φ accepts — i.e., a path with an infinitely-recurring accepting state in the product —
i.e., a reachable **cycle through an accepting state**. If no such cycle exists, no execution of M
can ever actually drive A_¬φ into accepting it, so no execution violates φ, so M ⊨ φ.

## 3. Complexity
**LTL model checking is PSPACE-complete in the size of the formula φ** — the Büchi automaton for φ
can be *exponentially* larger than φ itself, but the standard algorithm never builds it explicitly;
it searches for an accepting cycle **on-the-fly**, which keeps the space requirement polynomial
in |φ| even though the automaton it is implicitly searching is exponential. Model checking is
**polynomial in the size of the model M** (the product's cycle-detection step is a standard graph
algorithm, linear or near-linear in the product's size). In practice, the real industrial
bottleneck is **M's state-space size**, which can itself be astronomically large for a realistic
system (the so-called state-explosion problem) — addressed by techniques such as **symbolic/
BDD-based model checking**, which represents large sets of states compactly using Binary Decision
Diagrams (the same data structure named conceptually in Week 6) rather than enumerating states
individually. This is described conceptually here; this course's lab uses small, explicit,
enumerable state graphs.

## 4. A Teaching-Scale Explicit-State LTL Checker
This is a **simplified stand-in**, not the full automata-theoretic product construction: it
decides a useful LTL fragment (G, F, X, U, Boolean connectives) over a small explicit state graph
by direct search for a property-violating cycle, which is correct for this fragment at this scale
but is explicitly not the general Büchi-automaton algorithm.

```python
from dataclasses import dataclass

@dataclass
class Kripke:
    states: set
    initial: set
    trans: dict   # state -> set of successor states
    labels: dict  # state -> set of atomic propositions true there

def reachable(kripke, start_set):
    seen, frontier = set(start_set), list(start_set)
    while frontier:
        s = frontier.pop()
        for nxt in kripke.trans.get(s, ()):
            if nxt not in seen:
                seen.add(nxt)
                frontier.append(nxt)
    return seen

def check_safety_always_not(kripke, bad_prop):
    """Checks M |= G(not bad_prop): no reachable state has bad_prop true."""
    bad_states = {s for s in reachable(kripke, kripke.initial) if bad_prop in kripke.labels[s]}
    return len(bad_states) == 0, bad_states

def find_cycle_through(kripke, target_states, reachable_states):
    """Finds a reachable cycle passing through any state in target_states, via DFS."""
    visited, on_stack, stack_path = set(), set(), []

    def dfs(node):
        visited.add(node); on_stack.add(node); stack_path.append(node)
        for nxt in kripke.trans.get(node, ()):
            if nxt not in reachable_states:
                continue
            if nxt in on_stack and nxt in target_states:
                return stack_path[stack_path.index(nxt):] + [nxt]
            if nxt not in visited:
                result = dfs(nxt)
                if result:
                    return result
        on_stack.remove(node); stack_path.pop()
        return None

    for s in kripke.initial:
        if s not in visited:
            found = dfs(s)
            if found:
                return found
    return None

def check_liveness_req_eventually_resp(kripke, req_prop, resp_prop):
    """Checks M |= G(req -> F resp) by searching for a reachable cycle in which req holds
    somewhere but resp never holds anywhere on the cycle (a violating execution pattern)."""
    reach = reachable(kripke, kripke.initial)
    bad_cycle_states = {s for s in reach
                         if req_prop in kripke.labels[s] and resp_prop not in kripke.labels[s]}
    cycle = find_cycle_through(kripke, bad_cycle_states, reach)
    return (cycle is None), cycle
```

## 5. Worked Example: Agent Belief-Consistency
Model a toy agent's information states as a Kripke structure where each state's labels record
`believes_p` / `believes_not_p`. Checking the **safety** property `G(not (believes_p and
believes_not_p))` (the agent never simultaneously believes p and ¬p) reuses
`check_safety_always_not` with `bad_prop = "inconsistent"` pre-computed on any state where both
belief labels hold. Checking the **liveness** property `G(query_received -> F query_answered)`
(every query is eventually answered) uses `check_liveness_req_eventually_resp`. This is a direct,
concrete connection back to the graduate course's multi-agent epistemic logic: the same
Kripke-structure machinery now answers a *verification* question (does the specification hold on
every execution?) rather than an epistemic-truth question (does the agent know φ at this world?).

## 6. In-Class/Lab Exercise
Build a 5-state toy Kripke structure modeling a request/response protocol with one designed flaw
(a reachable cycle where `query_received` holds forever without `query_answered`). Run
`check_liveness_req_eventually_resp`, confirm it reports the property violated, and print the
returned cycle as a human-readable counterexample trace; then patch the transition relation to
remove the flaw and confirm the checker now reports the property holds.

# Week 9 — Lecture Content: Midterm Exam + Classical Planning

## 1. Midterm Exam
Covers Weeks 1–8 (AI foundations, agents, uninformed/informed/adversarial search, propositional
and first-order logic). See the Week 8 review roadmap for the topic list and practice problems.

## 2. Why Planning Is Different From General Search
Search (Weeks 3–5) treats actions as opaque: `result(state, action)` is a black box the search
algorithm calls but cannot inspect. **Classical planning** instead represents actions in a
structured way — their preconditions and effects are explicit — which lets a planner reason
about *which* actions are even relevant to a goal, rather than blindly generating successors.

## 3. The STRIPS Representation
A STRIPS action schema has:
- **Preconditions**: facts that must hold before the action can be applied.
- **Add-list**: facts the action makes true.
- **Delete-list**: facts the action makes false.

States and goals are represented as sets of facts (positive literals only, in the simplest
form).

```python
class StripsAction:
    def __init__(self, name, preconditions, add_list, delete_list):
        self.name = name
        self.preconditions = set(preconditions)
        self.add_list = set(add_list)
        self.delete_list = set(delete_list)

    def is_applicable(self, state):
        return self.preconditions.issubset(state)

    def apply(self, state):
        return (state - self.delete_list) | self.add_list

    def __repr__(self):
        return self.name
```

## 4. A Small Worked Example: Blocks World
Goal: stack block `A` on block `B`, given both start on the table and are clear.

```python
initial_state = {"on_table(A)", "on_table(B)", "clear(A)", "clear(B)"}
goal = {"on(A, B)"}

stack_A_on_B = StripsAction(
    name="Stack(A, B)",
    preconditions=["clear(A)", "clear(B)", "on_table(A)"],
    add_list=["on(A, B)"],
    delete_list=["on_table(A)", "clear(B)"],
)

if stack_A_on_B.is_applicable(initial_state):
    next_state = stack_A_on_B.apply(initial_state)
    print(goal.issubset(next_state))  # True: a 1-step plan solves this toy goal
```

## 5. Forward State-Space Planning Search
For non-trivial domains, the plan is found by searching over states, where each STRIPS action
applicable in the current state generates a successor — this is exactly BFS/UCS/A* from Weeks
3–4, now applied to a state space whose successors come from STRIPS action schemas rather than a
hand-written `result()` function.

```python
from collections import deque

def strips_forward_search(initial_state, goal, actions):
    frontier = deque([(frozenset(initial_state), [])])
    visited = {frozenset(initial_state)}
    while frontier:
        state, plan = frontier.popleft()
        if goal.issubset(state):
            return plan
        for action in actions:
            if action.is_applicable(state):
                next_state = frozenset(action.apply(set(state)))
                if next_state not in visited:
                    visited.add(next_state)
                    frontier.append((next_state, plan + [action]))
    return None
```

## 6. Why This Matters
STRIPS makes explicit exactly the structure a planner needs to check applicability and compute
effects without a problem-specific `result()` function — the same action schema can be reused
across many different initial states and goals in the same domain, which is the key advantage
over the domain-specific `Problem` classes from Weeks 3–5.

## 7. In-Class Exercise
Given a STRIPS action's preconditions, add-list, and delete-list and a state, determine by hand
whether the action is applicable and compute the resulting state; then extend the blocks-world
example to a 2-block-move plan.

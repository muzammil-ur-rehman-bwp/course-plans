# Week 2 — Lecture Content: Intelligent Agents

## 1. Agents and the Agent Function
An **agent** is anything that perceives its environment through sensors and acts upon it through
actuators. Formally, the **agent function** maps any percept history to an action:
`f: P* → A`. The **agent program** is the concrete implementation that runs on some physical
architecture and realizes that function. Our Python classes below are agent *programs*.

## 2. Agent Types
Agents are commonly classified by how much internal structure they use to decide on actions.

### Simple reflex agent
Acts purely on the current percept using condition-action rules; no memory of the past.
```python
def simple_reflex_vacuum_agent(location, status):
    if status == "Dirty":
        return "Suck"
    if location == "A":
        return "Right"
    if location == "B":
        return "Left"
```

### Model-based reflex agent
Maintains an internal **state** that tracks aspects of the world not directly observed, updated
using a model of how the world evolves.
```python
class ModelBasedVacuumAgent:
    def __init__(self):
        self.model = {"A": None, "B": None}  # internal belief about dirt status
        self.location = "A"

    def act(self, location, status):
        self.model[location] = status
        if status == "Dirty":
            return "Suck"
        if self.model["A"] == "Clean" and self.model["B"] == "Clean":
            return "NoOp"
        return "Right" if location == "A" else "Left"
```

### Goal-based agent
Chooses actions that lead toward an explicit **goal**, generally by considering the future
consequences of actions (this is where search, Weeks 3–5, becomes the decision mechanism).

### Utility-based agent
Chooses actions that maximize an explicit **utility function** over outcomes, allowing it to
trade off competing objectives (e.g., speed vs. safety) rather than just reaching *a* goal.

### Learning agent (brief note)
Any of the above can be paired with a **learning element** that improves the agent's performance
element over time from experience. This previews Weeks 12–13.

## 3. Environment Properties
How we should design an agent depends heavily on the properties of its task environment:

| Property | Meaning | Example |
|---|---|---|
| Fully vs. partially observable | Do sensors give complete state information? | Chess (full) vs. poker (partial — opponents' hands hidden) |
| Deterministic vs. stochastic | Is the next state fully determined by current state + action? | Chess (deterministic) vs. backgammon (stochastic, dice) |
| Episodic vs. sequential | Do episodes stand independent of each other? | Image classification (episodic) vs. chess (sequential) |
| Static vs. dynamic | Can the environment change while the agent deliberates? | Crossword (static) vs. taxi driving (dynamic) |
| Discrete vs. continuous | Are states/actions/time discrete or continuous? | Chess (discrete) vs. taxi driving (continuous) |
| Single-agent vs. multi-agent | Is there one agent, or do others act in the same environment? | Crossword (single) vs. chess (multi, competitive) |

### Worked classification
```python
environments = {
    "chess":          dict(observable="full", deterministic=True,  episodic=False,
                            static=True, discrete=True,  agents="multi"),
    "taxi driving":    dict(observable="partial", deterministic=False, episodic=False,
                            static=False, discrete=False, agents="multi"),
    "vacuum world":    dict(observable="partial", deterministic=True, episodic=False,
                            static=True, discrete=True,  agents="single"),
}
for name, props in environments.items():
    print(name, "->", props)
```

The harder an environment is along these axes (partially observable, stochastic, sequential,
dynamic, continuous, multi-agent), the more sophisticated the agent architecture generally needs
to be — this is why chess (fully observable, deterministic) was conquered by search decades
before open-world robotics.

## 4. Why This Matters for the Rest of the Course
Every later topic is, in this framework, a way of building a smarter agent program:
- **Search (Weeks 3–5)** builds goal-based and utility-based agents for environments where the
  agent can simulate the consequences of action sequences before acting.
- **Logic (Weeks 6–8)** gives agents a precise internal **model** (the knowledge-based agent)
  for environments where explicit, verifiable reasoning is required.
- **Planning (Week 9)** structures goal-based reasoning using an explicit action representation.
- **Probability (Weeks 10–11)** extends the model-based agent to partially observable, stochastic
  environments.

## 5. In-Class Exercise
Classify the taxi, crossword, poker, backgammon, and vacuum-world environments against the full
property table above and discuss any disagreements.

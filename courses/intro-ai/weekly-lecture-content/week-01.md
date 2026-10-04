# Week 1 — Lecture Content: Introduction to AI

## 1. What Is AI?
Russell & Norvig organize definitions of AI along two axes: whether we measure **thought** or
**behavior**, and whether we compare against **human** performance or an idealized **rational**
standard. This gives four views:

| | Human-centered | Rationality-centered |
|---|---|---|
| **Thought** | Thinking humanly (cognitive modeling) | Thinking rationally (logic-based reasoning) |
| **Behavior** | Acting humanly (Turing Test tradition) | Acting rationally (the "rational agent" view) |

This course takes the **acting rationally** view as its organizing principle: an intelligent
agent is one that, given what it has perceived and what it knows, acts to achieve the best
expected outcome. Nearly everything from Week 2 onward (agents, search, logic, planning,
probability) is a tool for building or analyzing rational agents.

## 2. A Short History of AI
A few load-bearing milestones:
- **1956** — The Dartmouth Workshop, where the term "Artificial Intelligence" was coined.
- **1950s–1960s** — Early search and reasoning programs (Logic Theorist, General Problem Solver).
- **1960s–1970s** — "Knowledge is power": expert systems that encode human expertise as rules.
- **1980s–1990s** — AI winters (overpromised results, underdelivered systems) alternating with
  renewed progress; probabilistic methods and Bayesian networks mature.
- **1997** — IBM's Deep Blue defeats world chess champion Garry Kasparov (search + evaluation
  functions at massive scale).
- **2010s** — Deep learning resurgence, driven by more data, more compute (GPUs), and improved
  training techniques.
- **2016** — DeepMind's AlphaGo defeats a top human Go player, combining deep neural networks
  with search.

The field has always had two intertwined threads: the **symbolic/classical** thread (search,
logic, planning — the core of this course, Weeks 2–11) and the **statistical/learning** thread
(machine learning, neural networks — surveyed in Weeks 12–15). Modern systems increasingly
combine both.

## 3. The Turing Test
Proposed by Alan Turing (1950) as an operational test for machine intelligence: a human judge
holds text conversations with a human and a machine, without knowing which is which; if the
judge cannot reliably tell them apart, the machine is said to have passed. It is a test of
**acting humanly**, not of rationality or genuine understanding.

**Common critiques:**
- A system could pass by clever mimicry without any internal understanding (the "Chinese Room"
  argument raises this concern).
- It rewards imitating human quirks and mistakes, which is not the same as rational behavior.
- It is a sufficient-but-not-necessary bar: a rational agent that acts nothing like a human
  (e.g., a calculator) can still be highly intelligent for its task.

We mention the Turing Test for historical completeness, but the rest of the course does not use
"indistinguishable from a human" as its definition of success — it uses **rational behavior
relative to a performance measure**, which brings us to PEAS.

## 4. The PEAS Framework
To design a rational agent we must first specify its **task environment** precisely. PEAS gives
four things to specify:
- **Performance measure** — how we judge success (not how the agent decides, but how an outside
  observer scores the outcome).
- **Environment** — what the agent operates in.
- **Actuators** — the means by which the agent acts.
- **Sensors** — the means by which the agent perceives.

### Worked example: Automated taxi
| PEAS element | Specification |
|---|---|
| Performance measure | Safety, speed, legality, passenger comfort, profit |
| Environment | Roads, other traffic, pedestrians, weather |
| Actuators | Steering, accelerator, brake, signal, horn, display |
| Sensors | Cameras, GPS, speedometer, accelerometer, engine sensors |

### Worked example: Vacuum-cleaning robot
| PEAS element | Specification |
|---|---|
| Performance measure | Amount of dirt cleaned, time taken, electricity used, noise |
| Environment | A room (or set of rooms) with furniture and dirt |
| Actuators | Wheels, brushes, vacuum suction |
| Sensors | Dirt sensor, bump sensor, (optionally) camera |

A representative PEAS specification in Python, as a plain data structure (no library needed —
this is a documentation tool, not a running algorithm):

```python
from dataclasses import dataclass, field

@dataclass
class PEAS:
    performance_measure: list[str]
    environment: list[str]
    actuators: list[str]
    sensors: list[str]

taxi = PEAS(
    performance_measure=["safety", "speed", "legality", "comfort", "profit"],
    environment=["roads", "other traffic", "pedestrians", "weather"],
    actuators=["steering", "accelerator", "brake", "signal", "horn"],
    sensors=["cameras", "GPS", "speedometer", "engine sensors"],
)
print(taxi)
```

Writing a PEAS specification is the first, essential step before choosing *any* algorithm later
in the course — it is impossible to pick a sensible search strategy, logical representation, or
planning approach without first knowing what the agent senses, does, and is judged on.

## 5. In-Class Exercise
In pairs, write a PEAS description for a chess-playing program. Compare: is the environment
fully or partially observable? Deterministic? These questions preview Week 2.

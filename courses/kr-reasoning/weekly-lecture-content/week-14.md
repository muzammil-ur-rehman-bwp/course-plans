# Week 14 — Lecture Content: Knowledge-Based Agents in Practice

## 1. Why Integrate?
Weeks 5 (rules) and 6 (frames/taxonomies) each solved a different representation problem in
isolation. A realistic knowledge-based agent needs both at once: taxonomic facts ("a sedan IS-A
car") feeding into rule premises ("IF x IS-A car AND x.fuel_level < 10 THEN x needs_fuel"). This
week builds one reasoner class that owns both a rule base and a frame hierarchy, and answers
queries by whichever chaining strategy fits, with an explanation trace attached to every answer.

## 2. A Unified Reasoner
```python
class KnowledgeBasedAgent:
    def __init__(self):
        self.facts = set()
        self.rules = []          # list of Rule(premises, conclusion), as in Week 5
        self.frames = {}         # name -> Frame, as in Week 6
        self.trace = {}          # derived fact -> justification string

    def add_fact(self, fact):
        self.facts.add(fact)
        self.trace[fact] = "given"

    def add_rule(self, rule):
        self.rules.append(rule)

    def add_frame(self, frame):
        self.frames[frame.name] = frame

    def _expand_taxonomic_facts(self):
        """Turn frame IS-A/slot knowledge into plain facts the rule engine can use as premises,
        e.g. a frame slot isa('sedan', 'car') becomes the fact 'isa(sedan,car)'."""
        for name, frame in self.frames.items():
            if frame.parent is not None:
                self.facts.add(f"isa({name},{frame.parent.name})")
            for slot, value in frame.slots.items():
                self.facts.add(f"{slot}({name},{value})")

    def ask_forward(self, query):
        self._expand_taxonomic_facts()
        facts = set(self.facts)
        changed = True
        while changed:
            changed = False
            for rule in self.rules:
                if rule.conclusion not in facts and rule.premises.issubset(facts):
                    facts.add(rule.conclusion)
                    self.trace[rule.conclusion] = f"rule({sorted(rule.premises)} -> {rule.conclusion})"
                    changed = True
        return query in facts, self.trace.get(query)

    def ask_backward(self, goal, visited=None):
        if visited is None:
            visited = set()
        self._expand_taxonomic_facts()
        if goal in self.facts:
            return True, "given"
        if goal in visited:
            return False, None
        visited.add(goal)
        for rule in self.rules:
            if rule.conclusion == goal:
                if all(self.ask_backward(p, visited)[0] for p in rule.premises):
                    justification = f"rule({sorted(rule.premises)} -> {goal})"
                    self.trace[goal] = justification
                    return True, justification
        return False, None

    def ask(self, query, strategy="forward"):
        if strategy == "forward":
            return self.ask_forward(query)
        return self.ask_backward(query)
```

## 3. Why a Derivation Trace Matters
A KR system that can only say "yes" or "no" to a query is far less useful than one that can say
*why*. The `trace` dict above records, for every derived fact, either `"given"` (it was an
original fact) or which rule fired to produce it. This directly supports **explainability** — one
of the three evaluation axes for KR systems the course returns to in Week 15 — and is exactly the
kind of feature real expert systems (e.g., MYCIN) were known for: not just an answer, but a
chain of reasoning a human expert could audit.

```python
def explain(agent, fact, depth=0):
    """Walk the trace backward, printing a human-readable derivation chain."""
    justification = agent.trace.get(fact)
    print("  " * depth + f"{fact}  <-  {justification}")
```

## 4. Worked Query on a Toy Diagnostic Domain
```python
agent = KnowledgeBasedAgent()
agent.add_fact("engine_wont_start")
agent.add_fact("no_clicking_sound")
agent.add_rule(Rule(frozenset({"engine_wont_start", "no_clicking_sound"}), "dead_battery"))
agent.add_rule(Rule(frozenset({"dead_battery"}), "needs_jumpstart"))

found, justification = agent.ask("needs_jumpstart", strategy="forward")
print(found, justification)
# True, "rule(['dead_battery'] -> needs_jumpstart)" -- and dead_battery's own trace entry
# in turn points back to the two given facts, forming a full chain an operator could audit.
```

## 5. In-Class Exercise
Extend the toy diagnostic domain with one frame-based fact (e.g., `Frame("sedan_123",
parent=Frame("car"))` with a slot `fuel_level = 5`) feeding a new rule's premise; query the
integrated agent and produce the full derivation trace for the result by hand.

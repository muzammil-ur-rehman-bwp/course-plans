# Week 6 — Lecture Content: Semantic Networks and Frames

## 1. Semantic Networks
A **semantic network** represents knowledge as a graph: nodes are objects or categories, and
labeled edges are relations — most importantly **IS-A** (subclass/instance-of) and **part-of**.
Properties attached to a category are intended to be **inherited** by everything below it in the
IS-A hierarchy — this is the network's main inferential payoff: state a property once, at the
most general level it holds, and every subtype/instance gets it "for free."

```python
class SemanticNetwork:
    def __init__(self):
        self.isa = {}        # child -> parent
        self.properties = {} # node -> {property: value}

    def add_isa(self, child, parent):
        self.isa[child] = parent

    def set_property(self, node, prop, value):
        self.properties.setdefault(node, {})[prop] = value

    def get_property(self, node, prop):
        """Strict inheritance: walk up the IS-A chain, return the first value found."""
        current = node
        while current is not None:
            if prop in self.properties.get(current, {}):
                return self.properties[current][prop]
            current = self.isa.get(current)
        return None
```

## 2. The Exceptions Problem
Set up the classic case: `Penguin` IS-A `Bird`; `Bird` has `can_fly = True`.

```python
net = SemanticNetwork()
net.add_isa("Tweety", "Penguin")
net.add_isa("Penguin", "Bird")
net.set_property("Bird", "can_fly", True)
print(net.get_property("Tweety", "can_fly"))  # True -- wrong! Penguins cannot fly.
```

**Strict inheritance gives the wrong answer**: `Tweety` inherits `can_fly = True` from `Bird`
because nothing at the `Penguin` level overrides it — the network has no mechanism yet to say
"all birds fly, *except* penguins." This is precisely the gap that motivates **non-monotonic
inheritance**: the conclusion "Tweety can fly" should be retractable once we know Tweety is a
penguin, even though strict (monotonic) inheritance, like strict logical entailment, cannot take
anything back once derived.

The fix is to let a more specific node's own property value take priority over an inherited one:

```python
net.set_property("Penguin", "can_fly", False)  # overrides the inherited Bird default
print(net.get_property("Tweety", "can_fly"))  # False -- correct, because get_property already
                                                # checks the node's own ancestors in order,
                                                # stopping at the first (most specific) match
```

The existing `get_property` already implements this correctly, because it walks from the most
specific node *upward*, stopping at the first value found — the fix was adding the override fact,
not changing the algorithm. This exception-overriding pattern is formalized properly as
**default logic** in Week 9; here it is introduced structurally, through the representation
itself.

## 3. Frames
A **frame** generalizes a semantic-network node into a richer structure: a named object or
category with **slots**, each slot holding a value, a **default value** (used only if no more
specific value is known), and optionally an attached procedure (a function computed on demand —
mentioned here for completeness, not required in this course's labs).

```python
class Frame:
    def __init__(self, name, parent=None):
        self.name = name
        self.parent = parent
        self.slots = {}          # slot -> value (specific to this frame)
        self.defaults = {}       # slot -> default value (specific to this frame)

    def set_slot(self, slot, value):
        self.slots[slot] = value

    def set_default(self, slot, value):
        self.defaults[slot] = value

    def get_slot(self, slot):
        """Resolve a slot: own value > own default > parent's resolution (recursively)."""
        if slot in self.slots:
            return self.slots[slot]
        frame = self
        while frame is not None:
            if slot in frame.slots:
                return frame.slots[slot]
            if slot in frame.defaults:
                return frame.defaults[slot]
            frame = frame.parent
        return None
```

### Worked Example: Frames Resolve Tweety Correctly
```python
bird = Frame("Bird")
bird.set_default("can_fly", True)       # a default, not a hard fact -- overridable

penguin = Frame("Penguin", parent=bird)
penguin.set_slot("can_fly", False)       # a specific fact that overrides the inherited default

tweety = Frame("Tweety", parent=penguin)
print(tweety.get_slot("can_fly"))        # False -- correctly resolved via Penguin's override
```

Because `get_slot` checks each frame's own `slots` *before* its `defaults`, and only then moves to
the parent, a more specific frame's explicit fact always wins over a more general frame's
default — this is exactly the non-monotonic behavior the plain semantic network above needed an
explicit extra fact to achieve, now built into the resolution algorithm itself.

## 4. Semantic Networks vs. Frames
A semantic network is naturally a graph of simple properties; a frame adds the default/explicit
distinction as a first-class concept, slot-level attached procedures, and a cleaner object-
oriented feel (closer to a class hierarchy in ordinary programming, which is why frames are often
described as a direct ancestor of object-oriented inheritance). Both still only support
single-chain IS-A inheritance here; multiple inheritance and conflicting defaults from two
parents are a known hard problem in frame systems, mentioned but not solved in this course.

## 5. In-Class Exercise
Extend the `Bird`/`Penguin`/`Tweety` frame hierarchy with `EmperorPenguin` (parent `Penguin`) that
sets no `can_fly` slot or default of its own. Trace by hand what `get_slot("can_fly")` returns for
an `EmperorPenguin` instance, and explain which frame's value it resolves to and why.

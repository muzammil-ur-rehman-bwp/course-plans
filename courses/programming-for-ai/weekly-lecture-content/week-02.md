# Week 2 — Lecture Content: Data Structures & OOP in Python

## 1. Built-in Data Structures
```python
nums = [1, 2, 3, 4]          # list: ordered, mutable
point = (3, 4)                # tuple: ordered, immutable
unique = {1, 2, 3}            # set: unordered, unique elements
ages = {"alice": 30, "bob": 25}  # dict: key-value mapping
```
| Structure | Ordered | Mutable | Typical AI use |
|---|---|---|---|
| list | yes | yes | sequences, batches, paths found by search |
| tuple | yes | no | fixed records, e.g., a (row, col) grid coordinate |
| set | no | yes | visited-node tracking in search algorithms |
| dict | yes (insertion) | yes | adjacency lists (graphs), feature name → value |

## 2. Comprehensions
```python
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
lengths = {word: len(word) for word in ["ai", "robot", "ml"]}
```
Comprehensions are idiomatic Python and frequently faster than an equivalent `for` loop with
`.append()`, because the loop is implemented in C internally.

## 3. Object-Oriented Programming Basics
```python
class GraphNode:
    def __init__(self, name):
        self.name = name
        self.neighbors = []

    def add_neighbor(self, node):
        self.neighbors.append(node)

    def __repr__(self):
        return f"GraphNode({self.name})"


class Graph:
    def __init__(self):
        self.nodes = {}

    def add_edge(self, a, b):
        self.nodes.setdefault(a, GraphNode(a))
        self.nodes.setdefault(b, GraphNode(b))
        self.nodes[a].add_neighbor(self.nodes[b])
```
Key ideas: `__init__` is the constructor; `self` refers to the instance; methods operate on
instance state. Inheritance (`class Dog(Animal):`) lets a subclass reuse/extend a parent class —
we will not need deep inheritance hierarchies in this course, but scikit-learn's estimator API and
PyTorch's `nn.Module` are both built around inheritance.

## 4. Worked Example: Building a Reusable `Graph` Class
We extend the `Graph` class above so it can be reused directly in Week 5's BFS/DFS labs — this is
the first concrete instance of "write code now that your future self will reuse."

## 5. In-Class Exercise
Refactor a given nested-loop that builds `{word: length}` for a list of words into a single dict
comprehension, and add a method to `Graph` that returns all neighbors of a given node.

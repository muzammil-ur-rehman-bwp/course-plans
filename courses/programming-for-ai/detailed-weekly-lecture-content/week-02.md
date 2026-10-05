# Week 2: Data Structures and Object-Oriented Programming in Python

## Learning Objectives

By the end of this lecture, you should be able to:

1. Choose between a list, tuple, set and dictionary for a given task, and justify the choice.
2. Write list, set and dictionary comprehensions and know when a plain loop is clearer.
3. Define classes with attributes and methods, and use special methods such as `__repr__`.
4. Explain the idea of inheritance and recognise it in scikit-learn and PyTorch.
5. Build a reusable `Graph` class that we will use again for search in Week 5.

## 1. Built-in Data Structures

Python ships with four containers that cover most needs. Picking the right one is mostly a question of what you will do with the data afterwards.

```python
nums = [1, 2, 3, 4]               # list: ordered, mutable
point = (3, 4)                    # tuple: ordered, immutable
unique = {1, 2, 3}                # set: unordered, unique elements
ages = {"alice": 30, "bob": 25}   # dict: key to value mapping

print(nums, point, unique, ages)
```

### 1.1 Lists

A list keeps items in order and can grow or shrink. It is the default choice when you have a sequence, such as a batch of samples or the path returned by a search.

```python
path = ["A", "B", "C"]
path.append("D")
path.insert(0, "start")
print(path)            # ['start', 'A', 'B', 'C', 'D']
print(path[1:3])       # ['A', 'B']
print(path[-1])        # 'D'
last = path.pop()
print(last, path)
```

Indexing a list by position is fast. Searching for a value with `in` is slow for long lists, because Python checks items one by one. This is a limit worth remembering.

### 1.2 Tuples

A tuple is like a list that cannot be changed after creation. That sounds like a restriction, but it is useful. Because tuples are immutable, they can be used as dictionary keys and set members, and they signal to the reader that the record is fixed.

```python
position = (2, 5)          # row, column
row, col = position        # unpacking
print(row, col)

visited = set()
visited.add(position)
print((2, 5) in visited)   # True
```

Grid coordinates in a maze are the standard example. A list `[2, 5]` cannot be added to a set, but the tuple `(2, 5)` can.

### 1.3 Sets

A set stores unique items and answers the question "have I seen this before?" very quickly, in roughly constant time.

```python
a = {1, 2, 3, 4}
b = {3, 4, 5}
print(a | b)    # union
print(a & b)    # intersection
print(a - b)    # difference

words = ["ai", "ml", "ai", "dl", "ml"]
print(set(words))   # duplicates removed
```

In search algorithms a set called `visited` prevents the program from exploring the same state twice.

### 1.4 Dictionaries

A dictionary maps keys to values and looks them up quickly. Since Python 3.7, dictionaries preserve insertion order.

```python
ages = {"alice": 30, "bob": 25}
ages["carol"] = 41
print(ages["alice"])
print(ages.get("dave", "unknown"))   # safe lookup

for name, age in ages.items():
    print(name, age)

counts = {}
for ch in "mississippi":
    counts[ch] = counts.get(ch, 0) + 1
print(counts)
```

The `get` method returns a default instead of raising `KeyError`. The counting loop above is a pattern you will write many times. The standard library offers shortcuts such as `collections.Counter` and `collections.defaultdict`.

```python
from collections import Counter, defaultdict

print(Counter("mississippi").most_common(2))

graph = defaultdict(list)
graph["A"].append("B")
graph["A"].append("C")
graph["B"].append("D")
print(dict(graph))
```

### 1.5 Choosing a structure

1. List: ordered sequences, batches, paths. Typical in AI code for sequences of tokens or states along a path.
2. Tuple: fixed records such as `(row, col)`, or a `(features, label)` pair.
3. Set: membership tests and removing duplicates, as in visited-node tracking.
4. Dictionary: lookups by name, such as adjacency lists for graphs or feature name to value.

A quick timing experiment makes the difference concrete.

```python
import time

n = 200_000
as_list = list(range(n))
as_set = set(as_list)
queries = range(n - 500, n + 500)   # worst case for the list

t0 = time.perf_counter()
hits = sum(1 for q in queries if q in as_list)
t_list = time.perf_counter() - t0

t0 = time.perf_counter()
hits2 = sum(1 for q in queries if q in as_set)
t_set = time.perf_counter() - t0

print(hits, hits2)
print(f"list: {t_list:.4f}s   set: {t_set:.6f}s")
```

On most machines the set finishes thousands of times faster. The exact numbers will differ on yours, but the direction will not.

## 2. Comprehensions

A comprehension builds a new collection from an existing one in a single expression.

```python
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
lengths = {word: len(word) for word in ["ai", "robot", "ml"]}
initials = {w[0] for w in ["apple", "avocado", "banana"]}

print(squares)
print(evens)
print(lengths)
print(initials)
```

The general form is `[expression for item in iterable if condition]`. Read it from the left: "give me this expression, for each item, but only if the condition holds".

Compare the loop version with the comprehension.

```python
words = ["ai", "robot", "ml", "vision"]

# loop version
lengths_loop = {}
for w in words:
    lengths_loop[w] = len(w)

# comprehension version
lengths_comp = {w: len(w) for w in words}

print(lengths_loop == lengths_comp)   # True
```

Comprehensions are often a little faster than the equivalent loop with `.append()`, because the iteration happens inside the interpreter's optimized code. More important for you is that they say what is being built, and not how.

### 2.1 When to stop

Nested comprehensions with several conditions become hard to read. If you need a comment to explain it, use a loop.

```python
# Fine
pairs = [(x, y) for x in range(3) for y in range(3) if x != y]
print(pairs)

# Getting heavy. A plain loop would be kinder to the next reader.
flat = [v for row in [[1, 2], [3, 4], [5]] for v in row if v % 2 == 1]
print(flat)
```

### 2.2 Generator expressions

Replace the square brackets with parentheses and you get a generator, which produces values one at a time instead of building the whole list in memory.

```python
total = sum(x * x for x in range(1_000_000))
print(total)
```

This matters when data is large, and it is the idea behind data loaders in deep learning.

## 3. Object-Oriented Programming Basics

A class bundles data and the functions that work on it. In AI code, classes appear whenever something has state that changes over time, such as a model with weights, a graph with nodes, or an environment with a current position.

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


g = Graph()
g.add_edge("A", "B")
g.add_edge("A", "C")
print(g.nodes["A"].neighbors)
```

Key ideas:

1. `__init__` is the constructor. It runs when you write `GraphNode("A")`.
2. `self` is the instance on which a method is called. Every method receives it as the first argument.
3. Attributes such as `self.neighbors` hold the state of that particular object.
4. `__repr__` controls how the object is printed. Without it you see an unhelpful memory address.

Notice that `add_edge` stores a directed edge from `a` to `b`. If you want an undirected graph you must add both directions. Making that choice explicit is part of the worked example below.

### 3.1 Inheritance

A subclass reuses and extends a parent class.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return "..."

    def describe(self):
        return f"{self.name} says {self.speak()}"


class Dog(Animal):
    def speak(self):
        return "woof"


class Cat(Animal):
    def speak(self):
        return "meow"


for pet in [Dog("Rex"), Cat("Tom"), Animal("Thing")]:
    print(pet.describe())
```

`describe` is written once in `Animal`, but it calls whichever `speak` belongs to the actual object. This is called polymorphism.

We will not build deep class hierarchies in this course. Even so, you need to recognise inheritance, since scikit-learn and PyTorch depend on it. A scikit-learn model is a class with `fit` and `predict` methods. A PyTorch network is a subclass of `nn.Module`. Writing your own model usually means inheriting from the framework's base class and filling in a few methods.

Here is a toy version of that idea.

```python
class MeanPredictor:
    """A deliberately simple 'model' that always predicts the training mean."""

    def fit(self, X, y):
        self.mean_ = sum(y) / len(y)
        return self

    def predict(self, X):
        return [self.mean_ for _ in X]


model = MeanPredictor().fit([[1], [2], [3]], [10, 20, 30])
print(model.predict([[4], [5]]))   # [20.0, 20.0]
```

The trailing underscore in `mean_` is a scikit-learn convention for values learned from data. Real estimators follow this same `fit` then `predict` shape, so learning it now makes Week 6 easier.

## 4. Worked Example: A Reusable Graph Class

In Week 5 we will run breadth-first and depth-first search on graphs. Rather than write throwaway code each week, we build one class now and keep it.

Requirements:

1. Add edges, either directed or undirected.
2. Return the neighbors of a node.
3. List all nodes and edges.
4. Print readably.

```python
class Graph:
    """A small graph stored as an adjacency dictionary."""

    def __init__(self, directed=False):
        self.directed = directed
        self.adj = {}

    def add_node(self, node):
        self.adj.setdefault(node, [])

    def add_edge(self, a, b):
        self.add_node(a)
        self.add_node(b)
        self.adj[a].append(b)
        if not self.directed:
            self.adj[b].append(a)

    def neighbors(self, node):
        return list(self.adj.get(node, []))

    def nodes(self):
        return list(self.adj)

    def edges(self):
        seen = set()
        result = []
        for a, nbrs in self.adj.items():
            for b in nbrs:
                key = (a, b) if self.directed else tuple(sorted((a, b)))
                if key not in seen:
                    seen.add(key)
                    result.append(key)
        return result

    def __repr__(self):
        kind = "directed" if self.directed else "undirected"
        return f"Graph({kind}, {len(self.adj)} nodes, {len(self.edges())} edges)"


g = Graph()
for a, b in [("A", "B"), ("A", "C"), ("B", "D"), ("C", "D"), ("D", "E")]:
    g.add_edge(a, b)

print(g)
print("neighbors of D:", g.neighbors("D"))
print("edges:", g.edges())
```

Design choices worth discussing:

1. We store nodes as plain keys rather than as separate node objects. For this course it is simpler, and the code stays short. The earlier `GraphNode` version is better if each node must carry extra data.
2. `neighbors` returns a copy of the list, so a caller cannot accidentally change the graph by editing the result.
3. `edges` uses a set to avoid reporting an undirected edge twice.

## 5. In-Class Exercise

Part A. Refactor this nested loop into one dictionary comprehension.

```python
words = ["tree", "graph", "node", "search"]
word_len = {}
for w in words:
    word_len[w] = len(w)

# your version
word_len2 = {w: len(w) for w in words}
print(word_len == word_len2)
```

Part B. Add a method to `Graph` that returns all neighbors of a given node. You may notice that `neighbors` above already does this, so extend the class with two further methods instead.

```python
def degree(self, node):
    return len(self.adj.get(node, []))

def has_edge(self, a, b):
    return b in self.adj.get(a, [])

Graph.degree = degree
Graph.has_edge = has_edge

print(g.degree("D"))          # 3
print(g.has_edge("A", "E"))   # False
print(g.has_edge("D", "E"))   # True
```

Attaching methods after the class is defined is only done here to keep the example short. In your own file, put them inside the class body.

Questions to think about:

1. What is the degree of each node in the example graph, and which node has the largest degree?
2. How would you change `has_edge` to run in constant time? (Hint: what if each adjacency entry were a set?)
3. How would you store edge weights?

## 6. Common Mistakes

1. Using a mutable object, such as a list, as a dictionary key. Use a tuple.
2. Forgetting `self` in a method definition or when accessing an attribute.
3. Writing `Graph.adj = {}` at class level and sharing one dictionary between all graphs. Create state inside `__init__`.
4. Copying a list with `b = a` and then being surprised that changing `b` changes `a`. Use `b = a.copy()` or `list(a)`.

```python
a = [1, 2, 3]
b = a
b.append(4)
print(a)       # [1, 2, 3, 4]
c = a.copy()
c.append(5)
print(a, c)
```

## 7. Summary

Lists, tuples, sets and dictionaries each have a clear role, and a good choice often decides whether code is fast or slow. Comprehensions express construction concisely, as long as they stay readable. Classes let us keep state and behaviour together, and inheritance is how scikit-learn and PyTorch are organised. The `Graph` class written today will be reused in Week 5.

## 8. Practice Problems

1. Given a list of words, build a dictionary that groups words by their first letter.
2. Write a `Stack` class with `push`, `pop`, `peek` and `is_empty`. Then write a `Queue` class using `collections.deque`. You will need both for DFS and BFS.
3. Extend `Graph` with a method `to_matrix()` that returns the adjacency matrix as a list of lists.
4. Create a class `Dataset` that holds a list of `(features, label)` tuples and supports `len()` and indexing by implementing `__len__` and `__getitem__`. This is the same interface PyTorch expects.

## 9. Suggested Reading

1. Python documentation on data structures (tutorial, section 5).
2. Python documentation on classes (tutorial, section 9).
3. The `collections` module documentation.

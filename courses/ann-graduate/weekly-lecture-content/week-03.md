# Week 3 — Lecture Content: Automatic Differentiation in Depth

## 1. Computational Graphs
Any numerical function built from elementary operations (add, multiply, sin, exp, ...) can be
represented as a **directed acyclic graph**: each node is either an input variable or the result
of applying one elementary operation to its parent nodes. Example, $f(x, y) = (x \cdot y) +
\sin(x)$:
$$
v_1 = x \cdot y, \qquad v_2 = \sin(x), \qquad f = v_1 + v_2
$$
Automatic differentiation (AD) computes exact derivatives of $f$ with respect to its inputs by
applying the chain rule systematically over this graph — it is neither symbolic differentiation
(which manipulates expressions and can blow up in size) nor numerical/finite-difference
differentiation (which is only approximate); AD is exact, up to floating-point rounding.

## 2. Forward-Mode AD
Forward mode propagates, alongside every node's value $v$, its **tangent**
$\dot v \equiv \partial v/\partial x$ for one chosen input $x$, computed by applying the chain
rule forward through the graph in the same order values are computed:
$$
\dot v_1 = \dot x \cdot y + x \cdot \dot y, \qquad \dot v_2 = \cos(x)\,\dot x, \qquad \dot f = \dot v_1 + \dot v_2
$$
Setting $(\dot x, \dot y) = (1, 0)$ computes $\partial f/\partial x$ in **one** forward sweep;
getting $\partial f/\partial y$ too requires a second sweep with $(\dot x,\dot y)=(0,1)$. In
general, forward mode needs one sweep **per input** to get the full gradient — efficient when a
function has few inputs and many outputs (e.g., a Jacobian with few columns).

## 3. Reverse-Mode AD
Reverse mode instead propagates, for a single scalar output $f$, each node's **adjoint**
$\bar v \equiv \partial f/\partial v$, computed backward from $f$ by summing over every node $w$
that $v$ feeds into:
$$
\bar v = \sum_{w \,:\, v \to w} \bar w\, \frac{\partial w}{\partial v}
$$
For the example above: $\bar f = 1$; $\bar v_1 = \bar f \cdot 1 = 1$, $\bar v_2 = \bar f \cdot 1 =
1$; then $\bar x = \bar v_1\cdot y + \bar v_2\cdot\cos(x)$ and $\bar y = \bar v_1 \cdot x$. **One**
backward sweep yields $\partial f/\partial x$ *and* $\partial f/\partial y$ simultaneously —
efficient when a function has many inputs (e.g., millions of network parameters) and one scalar
output (the loss), which is exactly the regime neural network training lives in. This asymmetry
is why every deep learning framework's `autograd`/`.backward()` uses reverse mode.

## 4. Backpropagation Is Reverse-Mode AD
A feedforward network's forward pass $a^{(l)} = g(W^{(l)}a^{(l-1)} + b^{(l)})$ is a computational
graph of affine-map and activation nodes with a single scalar loss output $L$. Reverse-mode AD's
general adjoint rule, specialized to this graph's structure, produces exactly the backpropagation
equations derived in the prerequisite course: the adjoint of the pre-activation node $z^{(l)}$ is
precisely $\delta^{(l)} = \partial L/\partial z^{(l)}$, and the general sum-over-children adjoint
rule becomes the layer-specific recursion $\delta^{(l)} = (W^{(l+1)\top}\delta^{(l+1)})\odot
g'(z^{(l)})$. Backpropagation is not a separate algorithm from automatic differentiation — it is
the one-sentence specialization "apply reverse-mode AD to a layered network's graph."

## 5. Code: A Minimal Reverse-Mode Autodiff Engine
```python
import math

class Value:
    """A node in a scalar computational graph with reverse-mode AD."""
    def __init__(self, data, _children=(), _op=""):
        self.data = data
        self.grad = 0.0
        self._backward = lambda: None
        self._prev = set(_children)
        self._op = _op

    def __add__(self, other):
        other = other if isinstance(other, Value) else Value(other)
        out = Value(self.data + other.data, (self, other), "+")
        def _backward():
            self.grad += out.grad
            other.grad += out.grad
        out._backward = _backward
        return out

    def __mul__(self, other):
        other = other if isinstance(other, Value) else Value(other)
        out = Value(self.data * other.data, (self, other), "*")
        def _backward():
            self.grad += other.data * out.grad
            other.grad += self.data * out.grad
        out._backward = _backward
        return out

    def sin(self):
        out = Value(math.sin(self.data), (self,), "sin")
        def _backward():
            self.grad += math.cos(self.data) * out.grad
        out._backward = _backward
        return out

    def tanh(self):
        t = math.tanh(self.data)
        out = Value(t, (self,), "tanh")
        def _backward():
            self.grad += (1 - t ** 2) * out.grad
        out._backward = _backward
        return out

    def backward(self):
        topo, visited = [], set()
        def build(v):
            if v not in visited:
                visited.add(v)
                for child in v._prev:
                    build(child)
                topo.append(v)
        build(self)
        self.grad = 1.0
        for v in reversed(topo):
            v._backward()

    __radd__ = __add__
    __rmul__ = __mul__

# Verify against the hand-worked example f(x, y) = x*y + sin(x)
x, y = Value(0.5), Value(0.8)
f = x * y + x.sin()
f.backward()
print(f"f={f.data:.6f}  df/dx={x.grad:.6f}  df/dy={y.grad:.6f}")
# Analytic check: df/dx = y + cos(x) ; df/dy = x
print(f"analytic  df/dx={0.8 + math.cos(0.5):.6f}  df/dy={0.5:.6f}")
```
The topological sort (`build`) guarantees every node's adjoint is fully accumulated — summed over
*all* of its children — before that node runs its own `_backward`, which is exactly the
sum-over-children rule from Section 3 and the reason shared sub-expressions must accumulate
gradients rather than overwrite them.

## 6. In-Class Exercise
Extend the engine with a `__pow__` method for integer powers and use it to verify
$d(x^3)/dx = 3x^2$ at $x=2$ by comparing `Value(2.0)**3` run through `.backward()` against the
analytic value.

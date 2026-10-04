# Week 1 — Lecture Content: Python Essentials & the AI Tooling Landscape

## 1. Why Python for AI?
Python dominates AI/ML development because of: readable syntax, a mature numerical computing
stack (NumPy, pandas), and first-class support from every major ML/deep-learning framework
(scikit-learn, PyTorch, TensorFlow). This course uses Python exclusively.

## 2. Variables, Types, Operators
```python
x = 5            # int
y = 3.14         # float
name = "AI"      # str
is_ready = True  # bool

print(x + y)       # 8.14 (arithmetic)
print(name * 2)    # "AIAI" (string repetition)
print(x > 3 and is_ready)  # boolean logic
```
Python is dynamically typed: a variable's type is determined at runtime by the value it holds.

## 3. Control Flow
```python
# Conditionals
score = 72
if score >= 85:
    grade = "A"
elif score >= 70:
    grade = "B"
else:
    grade = "C"

# Loops
total = 0
for i in range(10):
    total += i

n = 10
while n > 0:
    n -= 1
```

## 4. Functions
```python
def classify(score: float) -> str:
    """Return a letter grade for a numeric score."""
    if score >= 85:
        return "A"
    elif score >= 70:
        return "B"
    return "C"
```
Functions are first-class objects in Python — they can be passed as arguments, returned from
other functions, and stored in data structures. This is heavily used in ML code (e.g., passing a
loss function or activation function around).

## 5. The Python AI/ML Ecosystem
| Library | Role |
|---|---|
| NumPy | N-dimensional arrays, vectorized math — the foundation everything else builds on |
| pandas | Tabular data manipulation (loading, cleaning, EDA) |
| Matplotlib/Seaborn | Visualization |
| scikit-learn | Classical ML algorithms (regression, classification, clustering) with a unified API |
| PyTorch / TensorFlow-Keras | Deep learning: building and training neural networks |

A typical AI project pipeline: **load data (pandas) → explore/visualize (Matplotlib) →
preprocess/vectorize (NumPy/pandas) → train a model (scikit-learn or PyTorch/Keras) → evaluate →
iterate.**

## 6. Environment Setup
- Install Python 3.10+; create an isolated environment: `python -m venv venv` or `conda create`.
- Install packages: `pip install numpy pandas matplotlib scikit-learn jupyter`.
- Launch Jupyter: `jupyter notebook` (or use Google Colab for a zero-install option).

## 7. In-Class Exercise
Write a function `my_max(values: list) -> float` that returns the maximum value in a list
without calling the built-in `max()`. Discuss time complexity (O(n)).

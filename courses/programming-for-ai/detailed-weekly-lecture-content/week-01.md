# Week 1: Python Essentials and the AI Tooling Landscape

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain why Python became the main language for AI and machine learning work.
2. Use variables, basic types, operators, conditionals and loops with confidence.
3. Write functions with type hints and docstrings, and pass functions around as values.
4. Name the main libraries in the Python AI stack and say what each one is for.
5. Set up an isolated Python environment and run code in a script and in a notebook.

## 1. Why Python for AI?

Students often ask whether Python is the "best" language for AI. It is not the fastest, and it was not designed for numerical work. It became dominant for a more practical reason: the people who built the numerical libraries, and later the deep learning frameworks, chose Python as the front end, so almost every new method appears in Python first.

Several factors explain this.

1. The syntax is close to plain English and to pseudocode. A researcher can read a colleague's code and follow the idea without fighting the language.
2. The heavy computation does not actually run in Python. Libraries such as NumPy, PyTorch and TensorFlow are written in C, C++ and CUDA, and Python only describes what to compute.
3. The ecosystem is large and well maintained. For nearly any task, such as reading a CSV file, plotting a curve or training a classifier, a tested library already exists.
4. The community is big. When you hit an error, someone has almost certainly seen it before.

The cost of this convenience is speed in plain Python loops, which we will measure in Week 3. For now, accept that Python is the glue and the libraries are the engine.

This course uses Python only. All examples assume Python 3.10 or newer, though most will run on slightly older versions.

## 2. Variables, Types and Operators

A variable in Python is a name attached to a value. You do not declare its type. The type belongs to the value, and the same name can later point at a value of a different type. This is called dynamic typing.

```python
x = 5            # int
y = 3.14         # float
name = "AI"      # str
is_ready = True  # bool

print(x + y)               # 8.14
print(name * 2)            # AIAI
print(x > 3 and is_ready)  # True
print(type(x), type(y), type(name), type(is_ready))
```

Run this and read the output carefully. Adding an int and a float gives a float, because Python promotes the int. Multiplying a string by an integer repeats it. These small behaviours matter later, because NumPy follows similar promotion rules.

### 2.1 Common operators

1. Arithmetic: `+`, `-`, `*`, `/`, `//` (floor division), `%` (remainder), `**` (power).
2. Comparison: `==`, `!=`, `<`, `<=`, `>`, `>=`.
3. Logical: `and`, `or`, `not`.
4. Membership: `in` and `not in`, for example `3 in [1, 2, 3]`.

A classic early mistake is mixing up `/` and `//`.

```python
print(7 / 2)    # 3.5
print(7 // 2)   # 3
print(7 % 2)    # 1
print(2 ** 10)  # 1024
```

### 2.2 Dynamic typing: strength and trap

Dynamic typing makes code short, but it also lets a bug travel a long way before it shows up. Consider this.

```python
age = "25"
try:
    print(age + 1)
except TypeError as e:
    print("Error:", e)

print(int(age) + 1)   # convert first
```

The first call fails because Python will not silently add a string and a number. In real AI work this happens often when data is read from a file, since everything in a text file starts life as a string. Converting types deliberately is a habit worth building early.

## 3. Control Flow

### 3.1 Conditionals

```python
score = 72
if score >= 85:
    grade = "A"
elif score >= 70:
    grade = "B"
else:
    grade = "C"
print(grade)   # B
```

Python uses indentation, not braces, to mark blocks. Use four spaces and never mix tabs and spaces.

### 3.2 Loops

```python
total = 0
for i in range(10):
    total += i
print(total)   # 45

n = 10
steps = 0
while n > 0:
    n -= 1
    steps += 1
print(steps)   # 10
```

`range(10)` produces the numbers 0 to 9. The upper limit is excluded, which surprises beginners but makes slicing and indexing consistent.

Two small tools are very useful when looping.

```python
names = ["ann", "bilal", "chen"]
scores = [88, 74, 91]

for i, name in enumerate(names):
    print(i, name)

for name, score in zip(names, scores):
    print(f"{name} scored {score}")
```

`enumerate` gives you the position along with the item, and `zip` walks two lists together. You will use both in almost every data script you write.

### 3.3 Break, continue and the loop-else

```python
numbers = [4, 8, 15, 16, 23, 42]
for n in numbers:
    if n % 2 == 1:
        print("first odd number:", n)
        break
else:
    print("no odd numbers")
```

The `else` belongs to the loop and runs only if the loop finished without hitting `break`. It is rarely taught, but it is a clean way to express "search, and do something if nothing was found".

## 4. Functions

A function packages a piece of logic so that it can be reused and tested on its own.

```python
def classify(score: float) -> str:
    """Return a letter grade for a numeric score."""
    if score >= 85:
        return "A"
    elif score >= 70:
        return "B"
    return "C"

print(classify(90), classify(72), classify(40))
```

The annotations `score: float` and `-> str` are type hints. Python does not enforce them at run time, but editors and tools such as mypy use them to catch mistakes, and they document the function for the next reader. The text in triple quotes is a docstring and is available through `help(classify)`.

### 4.1 Default and keyword arguments

```python
def scale(values, factor=1.0, offset=0.0):
    return [v * factor + offset for v in values]

print(scale([1, 2, 3]))
print(scale([1, 2, 3], factor=2))
print(scale([1, 2, 3], offset=-1, factor=0.5))
```

Keyword arguments make calls readable, which matters when a model training function takes ten settings.

### 4.2 Functions are values

In Python a function is an object like any other. You can store it in a variable, put it in a list, or pass it to another function. This idea appears everywhere in machine learning code. A training loop takes a loss function as an argument. A neural network layer takes an activation function.

```python
import math

def relu(x):
    return max(0.0, x)

def sigmoid(x):
    return 1 / (1 + math.exp(-x))

def apply_all(func, values):
    return [func(v) for v in values]

data = [-2, -0.5, 0, 1, 3]
activations = {"relu": relu, "sigmoid": sigmoid}

for name, fn in activations.items():
    print(name, [round(v, 3) for v in apply_all(fn, data)])
```

Here `apply_all` does not care which function it receives. That is the point. In Week 12 you will see the same pattern when choosing activation functions for a network.

### 4.3 A short note on scope and mutable defaults

Variables created inside a function are local to it. A related trap is using a mutable object as a default argument.

```python
def add_item_bad(item, bucket=[]):
    bucket.append(item)
    return bucket

print(add_item_bad(1))
print(add_item_bad(2))   # [1, 2], not [2]

def add_item_good(item, bucket=None):
    if bucket is None:
        bucket = []
    bucket.append(item)
    return bucket

print(add_item_good(1))
print(add_item_good(2))  # [2]
```

The default list is created once, when the function is defined, and then shared across calls. Using `None` as the default avoids this.

## 5. The Python AI and ML Ecosystem

The libraries below form a stack. You will meet each of them in this course.

1. NumPy: N-dimensional arrays and fast vectorized mathematics. Almost everything else sits on top of it. (Week 3)
2. pandas: tabular data, for loading, cleaning and exploring. (Week 4)
3. Matplotlib and Seaborn: plotting. (Week 4 onward)
4. scikit-learn: classical machine learning, with one consistent interface for regression, classification and clustering. (Weeks 6 to 9)
5. PyTorch and TensorFlow with Keras: deep learning, where networks are built and trained with automatic differentiation. (Weeks 11 to 13)

A typical project follows this pipeline:

1. Load data with pandas.
2. Explore and visualize it with Matplotlib.
3. Preprocess it with NumPy and pandas.
4. Train a model with scikit-learn or a deep learning framework.
5. Evaluate the model honestly on data it has not seen.
6. Go back and iterate.

The last step is the one people forget to plan for. Real projects loop through the pipeline many times.

## 6. Environment Setup

Different projects need different versions of libraries. If you install everything globally, one project will eventually break another. A virtual environment keeps each project's packages separate.

On macOS or Linux:

```bash
python3 -m venv venv
source venv/bin/activate
pip install numpy pandas matplotlib scikit-learn jupyter
```

On Windows the activation command is `venv\Scripts\activate`.

Alternatively, with conda:

```bash
conda create -n ai python=3.11
conda activate ai
pip install numpy pandas matplotlib scikit-learn jupyter
```

To confirm the installation, save the following as `check_env.py` and run `python check_env.py`.

```python
import sys

print("Python:", sys.version.split()[0])
for lib in ["numpy", "pandas", "matplotlib", "sklearn"]:
    try:
        module = __import__(lib)
        print(f"{lib:12s} {module.__version__}")
    except ImportError:
        print(f"{lib:12s} NOT INSTALLED")
```

If you prefer not to install anything, Google Colab provides a ready environment in the browser. It is fine for the first few weeks, but you should learn to work locally too, because that is what you will do in a job or a research group.

### 6.1 Scripts versus notebooks

1. A script (`.py` file) runs from top to bottom. It is reproducible and easy to put under version control.
2. A notebook (`.ipynb`) mixes code, output and notes. It is excellent for exploration and teaching, but the hidden state between cells can mislead you. If results look strange, restart the kernel and run all cells in order.

A reasonable habit is to explore in a notebook and move stable code into `.py` files.

## 7. Worked Example: A Tiny Grade Report

This example uses only what we covered today. It reads a list of scores, classifies each one and prints a summary.

```python
def classify(score: float) -> str:
    if score >= 85:
        return "A"
    elif score >= 70:
        return "B"
    return "C"

students = {"Ann": 88, "Bilal": 74, "Chen": 91, "Dina": 62, "Eli": 70}

counts = {"A": 0, "B": 0, "C": 0}
for name, score in students.items():
    g = classify(score)
    counts[g] += 1
    print(f"{name:6s} {score:3d}  {g}")

average = sum(students.values()) / len(students)
print()
print("Grade counts:", counts)
print(f"Average score: {average:.1f}")
```

Expected output ends with the grade counts `{'A': 2, 'B': 2, 'C': 1}` and an average of 77.0. Notice how little code is needed, and notice that nothing here is specific to AI yet. The skill being practised is turning a small idea into clean, testable code.

## 8. In-Class Exercise

Write a function `my_max(values: list) -> float` that returns the largest value in a list without calling the built-in `max()`.

A starting point:

```python
def my_max(values: list) -> float:
    if not values:
        raise ValueError("my_max() needs a non-empty list")
    best = values[0]
    for v in values[1:]:
        if v > best:
            best = v
    return best

print(my_max([3, 9, 2, 7]))        # 9
print(my_max([-5, -2, -9]))        # -2
print(my_max([4.5]))               # 4.5
try:
    my_max([])
except ValueError as e:
    print("caught:", e)
```

Discussion points:

1. Why do we start with `values[0]` instead of `0`? Try the list `[-5, -2, -9]` if you are not sure.
2. The function looks at each element exactly once, so the time complexity is O(n). Doubling the list roughly doubles the work.
3. Could the answer be found faster on an unsorted list? Think about whether it is possible to skip any element.
4. What should the function do for an empty list? We chose to raise an error. Returning `None` is another defensible choice, and the right answer depends on how callers will use it.

## 9. Common Mistakes

1. Using `=` where `==` is intended in a condition.
2. Forgetting that `range(n)` stops at `n - 1`.
3. Modifying a list while looping over it.
4. Forgetting to activate the virtual environment, then wondering why an installed library cannot be imported.
5. Depending on notebook cells that were run out of order.

## 10. Summary

Python is popular in AI because it is readable and because its libraries do the heavy work in fast compiled code. The language basics, which are variables, control flow and functions, are the foundation for everything that follows. Treating functions as values is a habit that pays off in machine learning code. A clean, isolated environment saves a lot of trouble later.

## 11. Practice Problems

1. Write `count_vowels(text)` that returns the number of vowels in a string.
2. Write `fizzbuzz(n)` that returns a list of strings for the numbers 1 to n, using the usual rules.
3. Write `compose(f, g)` that returns a new function computing `f(g(x))`. Test it with `relu` and `sigmoid`.
4. Run `check_env.py` on your machine and record your library versions in a text file. You will be asked for them in later labs.

## 12. Suggested Reading

1. The official Python tutorial, sections 3 to 4.
2. Python documentation on `venv`.
3. Chapter 1 of any introductory text on Python for data analysis.

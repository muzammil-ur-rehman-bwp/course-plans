# Week 12 — Lecture Content: Recursion

## 1. The Two Required Parts
Every correct recursive method needs:
1. A **base case** — a condition simple enough to answer directly, without further recursive
   calls. This is what stops the recursion.
2. A **recursive case** — the method calls itself with a "smaller" version of the problem,
   progressing toward the base case.

## 2. Factorial — The Classic First Example
```java
static int factorial(int n) {
    if (n <= 1) {
        return 1;           // base case
    }
    return n * factorial(n - 1);   // recursive case
}
```
Tracing `factorial(4)`:
```
factorial(4) = 4 * factorial(3)
             = 4 * (3 * factorial(2))
             = 4 * (3 * (2 * factorial(1)))
             = 4 * (3 * (2 * 1))
             = 24
```
Each call waits for the one below it to return before it can compute its own result — this is
why recursive calls build up a chain of pending calls, all open at once.

## 3. The Call Stack
Each active method call gets its own stack frame, holding its own local variables and parameters
— this is the same stack model from Week 8, just with several frames for the *same* method
active simultaneously:
```
factorial(1) -> returns 1
factorial(2) -> waiting, then returns 2 * 1 = 2
factorial(3) -> waiting, then returns 3 * 2 = 6
factorial(4) -> waiting, then returns 4 * 6 = 24
```

## 4. Fibonacci
```java
static int fibonacci(int n) {
    if (n <= 1) {
        return n;        // base case: fib(0) = 0, fib(1) = 1
    }
    return fibonacci(n - 1) + fibonacci(n - 2);   // recursive case
}
```
Note this version makes **two** recursive calls per invocation — tracing `fibonacci(5)` by hand
reveals the same smaller subproblems (e.g., `fibonacci(2)`) get recomputed many times. This is a
useful, honest illustration that recursion is not automatically efficient; a loop-based version
that saves intermediate results computes the same answer far faster for larger `n`.

## 5. Sum of an Array, Recursively
```java
static int sumArray(int[] values, int index) {
    if (index == values.length) {
        return 0;                                     // base case: nothing left to add
    }
    return values[index] + sumArray(values, index + 1); // recursive case
}

// called as: sumArray(values, 0)
```
This pattern — an index parameter that advances toward a base case at the end of an array — comes
up repeatedly whenever you convert an array-processing loop into a recursive method.

## 6. Recursion vs. Iteration
Anything written recursively could also be written with a loop, and vice versa. Recursion is
often clearer for problems that are naturally self-similar (a smaller version of the same
problem) — tree/graph traversal in later courses is the prototypical example. A loop is usually
more efficient (no extra stack frames) and clearer for simple linear accumulation, as in Weeks 4
and 6. Choose based on which expresses the problem's structure more naturally, not out of habit.

## 7. `StackOverflowError`
```java
static int broken(int n) {
    return broken(n - 1);   // no base case at all — recurses forever
}
```
Calling `broken(5)` eventually throws a `StackOverflowError` — each call adds another frame to
the call stack, and the stack has a finite size. This is the recursive analog of an infinite
loop, and the fix is the same lesson as always: make sure every recursive call actually
progresses toward a base case that will be reached.

## 8. In-Class Exercise
Trace `factorial(3)` by hand, writing out each stack frame as it opens and the value it returns
as it closes. Then write `sumArray` from Section 5 and test it on an array of 5 integers,
confirming it matches a loop-based sum.

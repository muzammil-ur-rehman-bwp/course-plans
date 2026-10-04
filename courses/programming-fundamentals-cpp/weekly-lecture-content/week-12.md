# Week 12 — Lecture Content: Recursion

## 1. What Is Recursion?
A recursive function solves a problem by calling itself on a smaller version of the same problem.
Every correct recursive function needs:
1. A **base case** — the simplest input, solved directly without a further recursive call.
2. A **recursive case** — reduces the problem toward the base case and combines the result of the
   recursive call to produce the answer.

```cpp
int factorial(int n) {
    if (n <= 1) {          // base case
        return 1;
    }
    return n * factorial(n - 1);  // recursive case
}
```

## 2. Tracing the Call Stack
`factorial(4)` expands as:
```
factorial(4) = 4 * factorial(3)
factorial(3) = 3 * factorial(2)
factorial(2) = 2 * factorial(1)
factorial(1) = 1                    <- base case reached, stack starts unwinding
factorial(2) = 2 * 1 = 2
factorial(3) = 3 * 2 = 6
factorial(4) = 4 * 6 = 24
```
Each call waits on the stack for its recursive call to return before it can compute its own
result — the same call stack mechanism underlying every function call, just now with several
"pending" calls to the same function stacked on top of each other.

## 3. More Examples
```cpp
// Fibonacci: two base cases, two recursive calls
int fibonacci(int n) {
    if (n <= 1) return n;                       // base cases: fib(0)=0, fib(1)=1
    return fibonacci(n - 1) + fibonacci(n - 2);  // recursive case
}

// Sum of an array, recursively
int sumArray(const int values[], int size) {
    if (size <= 0) return 0;                           // base case: empty array
    return values[0] + sumArray(values + 1, size - 1);  // recursive case: first + sum of rest
}
```
`sumArray` demonstrates recursing over an array by advancing the pointer and shrinking the size —
the array-processing equivalent of "reduce toward the base case."

## 4. Recursion vs. Iteration
Every recursive function in this course could also be written iteratively (with a loop), and vice
versa. Recursion is often more directly readable for problems that are naturally defined in terms
of smaller versions of themselves (factorial, tree-shaped data, divide-and-conquer algorithms),
but each recursive call adds a frame to the call stack — too many nested calls (e.g., a missing or
incorrect base case) exhausts the stack and crashes the program (**stack overflow**):
```cpp
int broken(int n) {
    return n * broken(n - 1);  // BUG: no base case — recurses forever, eventually crashes
}
```

## 5. In-Class Exercise
Write a recursive function `int countDigits(int n)` that returns the number of digits in a
non-negative integer `n` (e.g., `countDigits(482)` returns 3), identify its base case and
recursive case, and trace it by hand for `n = 482`.

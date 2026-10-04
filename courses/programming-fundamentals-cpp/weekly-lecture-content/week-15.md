# Week 15 — Lecture Content: Debugging, Testing, and Program Design/Style

## 1. Compiler Warnings Are Free Bug Reports
```bash
g++ -std=c++17 -Wall -Wextra buggy.cpp -o buggy
```
```cpp
int average(int values[], int size) {
    int sum;              // -Wall flags: 'sum' may be used uninitialized
    for (int i = 0; i < size; ++i) {
        sum += values[i];
    }
    return sum / size;
}
```
`-Wall -Wextra` catches an entire class of bugs (uninitialized variables, comparing signed and
unsigned types, unused variables, suspicious implicit conversions) before the program ever runs.
**Treat every warning as a bug report** — fixing the warning (here, `int sum = 0;`) often fixes a
real, silent bug.

## 2. Using a Debugger
A debugger lets you pause execution and inspect the program's actual state, instead of guessing
from scattered `print` statements:
- **Breakpoint**: a line where execution pauses.
- **Step**: execute one line/call at a time.
- **Watch a variable**: see its value update live as the program runs.

```bash
g++ -std=c++17 -g buggy.cpp -o buggy   # -g includes debug info
gdb ./buggy
(gdb) break average
(gdb) run
(gdb) next
(gdb) print sum
```
The same workflow is available through any IDE's integrated debugger (e.g., VS Code's "Run and
Debug" panel) with breakpoints set by clicking the margin instead of typing commands.

## 3. Const-Correctness
```cpp
void printReport(const std::string& title, const int scores[], int size) {
    // title and scores are guaranteed, by the compiler, not to be modified here
    std::cout << title << ":\n";
    for (int i = 0; i < size; ++i) {
        std::cout << scores[i] << " ";
    }
}
```
Marking a parameter `const` is not decoration — it is a compiler-enforced promise. If the function
body accidentally tries to modify `scores[0]` or reassign `title`, the compiler refuses to
compile, catching the bug immediately rather than letting it corrupt data silently at runtime.
Apply `const` by default to any reference/pointer parameter the function only reads.

## 4. Defensive Programming
```cpp
double safeDivide(double numerator, double denominator) {
    if (denominator == 0.0) {
        std::cout << "Error: division by zero.\n";
        return 0.0;   // or signal failure some other agreed-upon way
    }
    return numerator / denominator;
}
```
Validate inputs at the boundary of a function rather than assuming callers always pass sensible
values — this is especially important once a program reads user input or file data (Week 11),
which can never be fully trusted to be well-formed.

## 5. A Basic Testing Mindset
Before formal unit-testing frameworks, build the habit of writing a few concrete input/expected-
output pairs for each function, and checking them by hand after every change:
```cpp
// Manual test cases for isPalindrome (Week 7)
// isPalindrome("Racecar") -> true
// isPalindrome("Hello")   -> false
// isPalindrome("")        -> true (edge case: empty string)
```
Writing these down *before* running the code forces you to think about edge cases (empty input,
single element, all-equal values) that are easy to forget when testing only "the happy path."

## 6. In-Class Exercise
Given a provided buggy program (off-by-one loop bound, an uninitialized variable, and a missing
`const`), use `-Wall -Wextra` and a debugger to find and fix all three issues, then write three
manual test cases confirming the fix.

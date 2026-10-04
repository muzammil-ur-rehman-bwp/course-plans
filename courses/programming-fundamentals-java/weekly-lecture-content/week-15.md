# Week 15 — Lecture Content: Debugging, Testing, and Program Design/Style

## 1. Reading a Stack Trace
```java
public class Buggy {
    public static void main(String[] args) {
        String[] names = new String[3];
        System.out.println(names[0].length());   // names[0] was never assigned — it's null
    }
}
```
Running this prints a **stack trace**:
```
Exception in thread "main" java.lang.NullPointerException:
    Cannot invoke "String.length()" because "names[0]" is null
        at Buggy.main(Buggy.java:4)
```
Read a stack trace from the top: the exception type (`NullPointerException`) tells you the kind
of problem; the message (in modern JDKs) often names the exact null expression; and
`at Buggy.main(Buggy.java:4)` gives the exact file and line where it happened. For a chain of
calls, each `at ...` line below the first shows who called whom, deepest call first — the bug is
almost always at or very near the top-most `at` line in your own code.

## 2. Common Exceptions and Their Typical Causes
| Exception | Typical cause |
|---|---|
| `NullPointerException` | Using a reference that is `null` (an uninitialized field, a failed lookup) |
| `ArrayIndexOutOfBoundsException` | An off-by-one loop bound, or an index computed incorrectly |
| `NumberFormatException` | Parsing text that isn't a valid number (`Integer.parseInt("abc")`) |
| `ArithmeticException` | Integer division by zero (`5 / 0`) — note floating-point division by zero instead produces `Infinity`/`NaN`, not an exception |

## 3. Using a Debugger
Rather than adding and removing `System.out.println` statements to inspect values (a valid but
slow technique), an IDE debugger (IntelliJ IDEA or VS Code) lets you:
1. Set a **breakpoint** on a line — execution pauses there.
2. **Step** through code line by line (`step over`, `step into`, `step out`).
3. **Watch** variables' values as they change, and inspect the call stack at the paused point.

This is far faster for understanding *why* a value is wrong than guessing and re-running.

## 4. Defensive Programming: Input Validation and `assert`
```java
static double average(int[] values) {
    if (values == null || values.length == 0) {
        throw new IllegalArgumentException("values must be non-null and non-empty");
    }
    int total = 0;
    for (int v : values) {
        total += v;
    }
    double result = (double) total / values.length;
    assert result >= 0 || hasNegatives(values) : "average should not be negative without negative inputs";
    return result;
}
```
Validating arguments at the top of a method (and failing loudly with a clear exception) catches
misuse immediately, at the source, rather than letting a bad value silently propagate and cause a
confusing failure somewhere else later. `assert` statements document an expected internal
invariant; they are disabled by default at run time (enabled with the `-ea` JVM flag), so they
supplement but never replace real input validation.

## 5. A Basic Unit-Testing Mindset
Before using a formal framework like JUnit (introduced in later courses), you can still test
systematically by hand:
```java
public class ManualTests {
    public static void main(String[] args) {
        test(factorial(0) == 1, "factorial(0) should be 1");
        test(factorial(1) == 1, "factorial(1) should be 1");
        test(factorial(5) == 120, "factorial(5) should be 120");
    }

    static void test(boolean condition, String description) {
        System.out.println((condition ? "PASS: " : "FAIL: ") + description);
    }

    static int factorial(int n) {
        if (n <= 1) return 1;
        return n * factorial(n - 1);
    }
}
```
Writing down expected outputs *before* running code, and checking every boundary case (zero,
negative, the largest/smallest value a loop should handle), is the same discipline formal testing
frameworks later automate.

## 6. Refactoring for Style and Encapsulation
Good style habits reinforced this week: meaningful variable/method names, one clear responsibility
per method, consistent indentation, and (from Week 13) exposing only what a class needs to —
private fields with validated access, not public mutable data.

## 7. In-Class Exercise
Given a provided buggy program, read its stack trace to form a hypothesis about the bug's
location and cause, then open it in the debugger, set a breakpoint near that line, and step
through to confirm (or revise) the hypothesis before fixing it.

# Week 12 — Lecture Content: Exception Handling

## 1. `try`/`catch`/`throw`
```java
public class Divider {
    public static int divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero");
        }
        return a / b;
    }
}

try {
    int result = Divider.divide(10, 0);
} catch (ArithmeticException e) {
    System.out.println("Error: " + e.getMessage());   // Error: Cannot divide by zero
}
```
`throw` raises an exception object; a `try` block's code that might fail is wrapped, and a
matching `catch` block handles the exception instead of letting the program crash.

## 2. The Exception Hierarchy: Checked vs. Unchecked
```java
// Throwable
//   ├── Error                (serious JVM problems — not normally caught)
//   └── Exception
//         ├── RuntimeException (and subclasses)   — UNCHECKED
//         └── everything else                      — CHECKED
```
```java
import java.io.IOException;

public void readFile(String path) throws IOException {   // CHECKED — must be declared or caught
    // ... code that might throw IOException ...
}

public void divide(int a, int b) {
    if (b == 0) {
        throw new IllegalArgumentException("b cannot be 0");  // UNCHECKED — no "throws" required
    }
}
```
A **checked** exception (anything extending `Exception` but not `RuntimeException`) must be
either caught or declared with `throws` in the method signature — the compiler enforces this,
a discipline C++ leaves entirely to convention. An **unchecked** exception (extending
`RuntimeException`, like `IllegalArgumentException` or `NullPointerException`) needs neither;
it signals a programming error the caller isn't forced to anticipate everywhere.

## 3. Writing a Custom Exception Class
```java
public class InsufficientFundsException extends Exception {   // checked — caller must handle it
    public InsufficientFundsException(String message) {
        super(message);
    }
}

public class BankAccount {
    private double balance;

    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(
                "Requested " + amount + " but balance is only " + balance);
        }
        balance -= amount;
    }
}
```
```java
BankAccount acc = new BankAccount();
try {
    acc.withdraw(500);
} catch (InsufficientFundsException e) {
    System.out.println("Withdrawal failed: " + e.getMessage());
}
```
Extending `Exception` makes a custom exception checked (callers of `withdraw` must handle or
re-declare it); extending `RuntimeException` instead would make it unchecked. The choice should
reflect whether callers can reasonably be expected to recover from the condition (checked) or
whether it signals a bug to fix in the code, not handle at every call site (unchecked).

## 4. Multiple `catch` Clauses, Specific to General
```java
try {
    acc.withdraw(500);
    riskyParse();
} catch (InsufficientFundsException e) {
    System.out.println("Funds problem: " + e.getMessage());
} catch (NumberFormatException e) {
    System.out.println("Parse problem: " + e.getMessage());
} catch (Exception e) {            // catch-all MUST come last — it matches everything above too
    System.out.println("Unexpected: " + e.getMessage());
}
```
Catch clauses are checked top to bottom; a more general type (like plain `Exception`) placed
before a more specific one would make the specific clause unreachable, which the compiler flags
as an error. Order from most specific to most general.

## 5. try-with-resources
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public void printFirstLine(String path) throws IOException {
    try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
        System.out.println(reader.readLine());
    }   // reader.close() is called automatically here, even if an exception was thrown above
}
```
Any resource implementing `AutoCloseable` can be declared inside a `try (...)` header; Java
guarantees it is closed when the block exits, whether normally or via an exception — replacing a
manual `finally { reader.close(); }` block and the bugs (forgetting the close, or closing the
wrong variable) that pattern invites.

## 6. In-Class Exercise
Write a checked `InvalidAgeException`, and a `Person` class whose constructor throws it (declared
with `throws`) when given a negative age. Write a caller that constructs several `Person` objects
inside a `try` block with multiple `catch` clauses, handling `InvalidAgeException` specifically
and any other `Exception` generically.

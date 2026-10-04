# Week 12 — Lecture Content: Exception Handling

## 1. Why Exceptions?
```cpp
double divide(double a, double b) {
    if (b == 0) {
        return -1;   // what if -1 is also a valid result? the caller can't tell error from data
    }
    return a / b;
}
```
Signaling errors through a special return value works only if every possible valid result can be
told apart from every possible error — often it can't. **Exceptions** separate error signaling
from the function's normal return value entirely: a function that cannot do its job can `throw`
an exception object instead of returning anything, and that exception propagates up the call
stack until something handles it.

## 2. `throw`, `try`, `catch`
```cpp
double divide(double a, double b) {
    if (b == 0) {
        throw std::runtime_error("division by zero");   // throw: stop normal execution here
    }
    return a / b;
}

int main() {
    try {
        std::cout << divide(10, 0) << "\n";   // never reached once divide() throws
    } catch (const std::runtime_error& e) {
        std::cout << "Error: " << e.what() << "\n";   // e.what() returns the message
    }
    std::cout << "Program continues normally.\n";
}
```
Code that might fail goes in a `try` block. When a `throw` happens anywhere inside it (even deep
inside a function it calls), execution jumps immediately to the first matching `catch` clause,
skipping everything else in the `try` block and in any function calls in between.

## 3. Stack Unwinding
```cpp
void level3() { throw std::runtime_error("failure at level 3"); }
void level2() { level3(); /* level3's exception just passes through level2 */ }
void level1() {
    try {
        level2();
    } catch (const std::runtime_error& e) {
        std::cout << "Caught: " << e.what() << "\n";
    }
}
```
When `level3()` throws, the runtime unwinds the call stack — destructors for every local object
in `level3()` and `level2()` run automatically, in reverse order, exactly as if those functions
had returned normally — until a matching `catch` is found (in `level1()` here) or the program
terminates if none is. This automatic cleanup is exactly RAII (destructors run no matter how a
scope is exited) applied to the error-handling path, previewing Week 15.

## 4. The Standard Exception Hierarchy
```cpp
#include <stdexcept>

// std::exception                      (base of all standard exceptions; has what())
//   ├── std::logic_error
//   │     ├── std::invalid_argument    (bad argument value)
//   │     └── std::out_of_range        (index/key outside valid range)
//   └── std::runtime_error
//         └── (your custom exceptions can derive here too)

void setAge(int age) {
    if (age < 0 || age > 150) {
        throw std::invalid_argument("age out of plausible range");
    }
}

void getElement(const std::vector<int>& v, std::size_t i) {
    if (i >= v.size()) {
        throw std::out_of_range("index beyond vector size");
    }
    std::cout << v[i] << "\n";
}
```
The `<stdexcept>` header provides a small hierarchy of ready-made exception types, all deriving
(directly or indirectly) from `std::exception`. Throwing the most specific standard type that
fits (`std::invalid_argument` for a bad argument, `std::out_of_range` for an index/key problem)
gives callers useful information about *what kind* of error occurred, which they can act on by
catching that specific type.

## 5. Writing a Custom Exception Class
```cpp
class InsufficientFundsError : public std::runtime_error {
public:
    InsufficientFundsError(double shortfall)
        : std::runtime_error("insufficient funds"), shortfall_(shortfall) {}

    double getShortfall() const { return shortfall_; }

private:
    double shortfall_;
};

class Account {
public:
    void withdraw(double amount) {
        if (amount > balance_) {
            throw InsufficientFundsError(amount - balance_);
        }
        balance_ -= amount;
    }
private:
    double balance_ = 0;
};

try {
    account.withdraw(100);
} catch (const InsufficientFundsError& e) {
    std::cout << e.what() << " — short by " << e.getShortfall() << "\n";
}
```
A custom exception class typically derives from `std::exception` (directly or, as here, through
`std::runtime_error`) and can add its own data (`shortfall_`) and methods beyond the inherited
`what()`. Deriving from an existing standard exception type means code that only knows how to
catch `std::exception` (or `std::runtime_error`) still catches your custom type correctly, thanks
to polymorphism (Week 8) — while code that specifically wants the extra detail can catch
`InsufficientFundsError` by name.

## 6. Catching Correctly: By Reference, Ordered, With a Catch-All
```cpp
try {
    account.withdraw(500);
} catch (const InsufficientFundsError& e) {    // most specific first
    std::cout << "Specific: " << e.what() << "\n";
} catch (const std::exception& e) {             // more general next
    std::cout << "General: " << e.what() << "\n";
} catch (...) {                                  // catch-all: anything not derived from exception
    std::cout << "Unknown error.\n";
}
```
`catch` clauses are tried **in order**, top to bottom — list the most specific exception types
first, more general ones after, or the general clause will catch everything before a more
specific one ever gets a chance (and some compilers warn about this unreachable-catch mistake).
Catch by **reference** (`const std::exception& e`), never by value: catching by value invokes the
exception's copy constructor and, worse, **slices** a derived exception object down to the base
type — exactly the slicing bug from Week 8, here losing access to `getShortfall()` and even
calling the wrong (base) `what()` if it were overridden. `catch (...)` matches anything at all,
including types unrelated to `std::exception`, and is normally used only as a last-resort
safety net, since it gives no information about what was actually caught.

## 7. In-Class Exercise
Extend the `Account` class with a `deposit(double amount)` method that throws
`std::invalid_argument` for a non-positive amount, and write a `try`/`catch` block in `main` that
exercises both `withdraw` (triggering `InsufficientFundsError`) and `deposit` (triggering
`std::invalid_argument`) with ordered `catch` clauses for each.

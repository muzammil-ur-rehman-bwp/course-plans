# Week 13 — Lecture Content: Intro to Classes

> This week previews just enough of C++ classes to prepare you for the dedicated Object-Oriented
> Programming course. We cover encapsulation, constructors, and member functions — not
> inheritance or polymorphism, which belong to that later course.

## 1. From `struct` to `class`
```cpp
struct PointStruct {
    int x;   // public by default
    int y;
};

class PointClass {
public:
    int x;   // explicitly marked public
    int y;
};
```
`struct` and `class` are nearly identical in C++ — the only default difference is that `struct`
members are `public` unless stated otherwise, while `class` members are `private` unless stated
otherwise. By convention, `struct` is used for simple data bundles (Week 10) and `class` is used
when a type also has private data and behavior (methods) protecting that data.

## 2. Encapsulation: Why Hide Data?
```cpp
class BankAccount {
private:
    double balance;   // hidden — cannot be touched directly from outside the class

public:
    BankAccount(double startingBalance) {
        if (startingBalance < 0) {
            balance = 0;   // enforce a rule: never start negative
        } else {
            balance = startingBalance;
        }
    }

    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    bool withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;   // reject an invalid withdrawal instead of corrupting balance
    }

    double getBalance() const {   // const: this method does not modify the object
        return balance;
    }
};
```
If `balance` were `public`, any code anywhere could set it to a negative number or skip the
withdrawal check entirely. **Encapsulation** — making data `private` and exposing only controlled
`public` methods — lets the class enforce its own rules every time its data changes, no matter who
is using it.

## 3. Constructors
```cpp
int main() {
    BankAccount account(100.0);   // constructor runs automatically, sets balance = 100.0
    account.deposit(50.0);
    account.withdraw(30.0);
    std::cout << account.getBalance() << "\n";  // 120
}
```
A **constructor** is a special member function, named exactly like the class, that runs
automatically when an object is created — it is the natural place to validate and set up initial
state (here, rejecting a negative starting balance).

## 4. Member Functions (Methods)
A member function is declared inside the class and can access the object's private data directly
(no getter needed from inside the class itself). `deposit`, `withdraw`, and `getBalance` above are
all member functions of `BankAccount`. Marking a method `const` (as `getBalance` is) promises the
compiler — and any reader — that calling it will not modify the object.

## 5. Objects Are Just Variables of a Class Type
```cpp
BankAccount accounts[3] = {
    BankAccount(100.0),
    BankAccount(250.0),
    BankAccount(0.0)
};

double total = 0;
for (int i = 0; i < 3; ++i) {
    total += accounts[i].getBalance();
}
```
An array of class-typed objects behaves exactly like the array-of-structs pattern from Week 10 —
the difference is that each `BankAccount` now protects and manages its own `balance` rather than
exposing it directly.

## 6. In-Class Exercise
Design and implement a class `Rectangle` with private `width` and `height`, a constructor that
rejects non-positive dimensions (clamping to 1 instead), and public methods `area()` and
`perimeter()`; verify it with a few test objects.

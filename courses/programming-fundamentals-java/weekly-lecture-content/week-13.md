# Week 13 — Lecture Content: More on Classes — Encapsulation, Static vs. Instance

## 1. Why Encapsulation?
```java
public class BadAccount {
    public double balance;   // public field — ANY code anywhere can set this to anything
}

BadAccount acc = new BadAccount();
acc.balance = -500.0;   // nothing stops an invalid, nonsensical balance
```
When a class's fields are `public`, nothing enforces the rules that should govern them (e.g., "a
balance should never go negative"). **Encapsulation** means making fields `private` and exposing
controlled access through public methods, so the class itself can enforce its own rules.

## 2. A Properly Encapsulated Class
```java
public class BankAccount {
    private double balance;   // private: only code inside this class can touch it directly

    public BankAccount(double initialBalance) {
        if (initialBalance < 0) {
            throw new IllegalArgumentException("Initial balance cannot be negative");
        }
        this.balance = initialBalance;
    }

    public double getBalance() {        // getter
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {   // setter-like method with validation
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;   // rejected: invalid amount or insufficient funds
    }
}
```
```java
BankAccount acc = new BankAccount(100.0);
acc.deposit(50.0);
boolean ok = acc.withdraw(1000.0);   // false — rejected, insufficient funds
System.out.println(acc.getBalance()); // 150.0 — the rejected withdrawal never took effect
```
Every change to `balance` now passes through code that can validate it — `balance` can never go
negative through normal use of this class, because no code outside the class can touch it
directly.

## 3. `static` vs. Instance Members
```java
public class BankAccount {
    private static int accountCount = 0;   // static: ONE copy, shared by the whole class
    private double balance;                 // instance: a SEPARATE copy per object

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
        accountCount++;             // every new account increments the single shared counter
    }

    public static int getAccountCount() {   // static method: no particular object needed
        return accountCount;
    }
}
```
```java
BankAccount a = new BankAccount(100.0);
BankAccount b = new BankAccount(200.0);
System.out.println(BankAccount.getAccountCount());   // 2 — called on the CLASS, not an instance
```
- An **instance field** (`balance`) exists once *per object* — `a` and `b` each have their own.
- A **static field** (`accountCount`) exists once *per class*, shared by every instance — exactly
  the same relationship `main` (static) has always had to the class it lives in, from Week 1.
- A **static method** (`getAccountCount`) is called on the class itself (`BankAccount.getAccountCount()`),
  not on a particular object, and can only directly access static fields — it has no `this`.

## 4. Choosing Getters and Setters Deliberately
Not every private field needs both a getter and a setter. `BankAccount` above deliberately has no
`setBalance(double)` — the *only* way to change `balance` is through `deposit`/`withdraw`, which
enforce the account's rules. A blanket "getter and setter for every field" habit defeats the
purpose of encapsulation if the setter has no validation at all.

## 5. Scope Note: Still Not Full OOP
This week deepens Week 9's introduction to classes with proper encapsulation and `static`
members, but **inheritance, polymorphism, interfaces, and `abstract` classes remain out of
scope** for this course — they are the subject of the dedicated Object-Oriented Programming
course that follows.

## 6. In-Class Exercise
Add a `private static int accountCount` field to a `BankAccount` class, increment it in the
constructor, and add a `public static int getAccountCount()` method. Create three accounts and
print the total count, calling the method on the class name, not on any one instance.

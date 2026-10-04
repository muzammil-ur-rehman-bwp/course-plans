# Week 4 — Lecture Content: Static vs. Instance Members

## 1. Instance Fields: One Copy Per Object
```java
public class Account {
    private double balance;   // instance field: EACH Account object has its own copy

    public Account(double balance) {
        this.balance = balance;
    }
}

Account a = new Account(100.0);
Account b = new Account(200.0);
// a.balance and b.balance are two completely separate pieces of memory
```

## 2. Static Fields: One Copy Per Class
```java
public class Account {
    private static int accountCount = 0;   // static: ONE copy, shared by the whole class
    private double balance;                // instance: a separate copy per object

    public Account(double balance) {
        this.balance = balance;
        accountCount++;   // every new Account increments the single shared counter
    }

    public static int getAccountCount() {   // static method: called on the CLASS
        return accountCount;
    }
}
```
```java
Account a = new Account(100.0);
Account b = new Account(200.0);
System.out.println(Account.getAccountCount());   // 2 — called on the class name, not an object
```
A **static field** exists once per class, shared by every instance — exactly the relationship
`main` (always `static`) has always had to the class it lives in. A **static method** is called on
the class itself, needs no particular object to run, and — because it has no `this` — can only
directly access other `static` members, never an instance field like `balance`.

## 3. Static Initialization
```java
public class Config {
    private static final String VERSION = "1.0";          // static field initializer — runs once

    private static final java.util.Map<String, String> DEFAULTS;
    static {                                                // static initializer block
        DEFAULTS = new java.util.HashMap<>();
        DEFAULTS.put("timeout", "30");
        DEFAULTS.put("retries", "3");
    }
}
```
A static field initializer (`VERSION`'s `= "1.0"`) and a static initializer block (`static { ... }`)
both run exactly once, when the class is first loaded by the JVM — before any instance of the
class is ever created, and regardless of whether one ever is. Use a block when the initialization
logic needs more than a single expression, as the `DEFAULTS` map does above.

## 4. Static Methods Have No `this`
```java
public class MathHelper {
    public static int square(int n) {
        return n * n;          // fine — only uses its own parameter
    }

    private int instanceField = 10;

    public static int broken() {
        return instanceField;  // COMPILE ERROR: cannot reference an instance field from a static context
    }
}
```
A static method has no receiving object, so there is no `this` and no instance field to read —
the compiler rejects any attempt to use one directly from a static method.

## 5. Utility Classes
```java
public final class MathUtils {
    private MathUtils() { }   // private constructor: prevents anyone from writing `new MathUtils()`

    public static double average(double a, double b) {
        return (a + b) / 2.0;
    }

    public static double clamp(double value, double min, double max) {
        if (value < min) return min;
        if (value > max) return max;
        return value;
    }
}

double avg = MathUtils.average(3.0, 7.0);   // called on the class — no instance ever needed
```
A **utility class** holds only `static` methods and is never meant to be instantiated — exactly
how `java.lang.Math` itself is built. The convention is a `private` (or in a `final` class, simply
unused) constructor, so attempting `new MathUtils()` fails to compile, signaling clearly that the
class exists only to group related static methods.

## 6. In-Class Exercise
Add a `private static int instanceCount` field to a `Student` class, increment it in every
constructor, and add `public static int getInstanceCount()`. Separately, write a static utility
class `Grades` with a private constructor and a static method `letterGrade(double score)` that
returns `"A"`/`"B"`/`"C"`/`"D"`/`"F"`.

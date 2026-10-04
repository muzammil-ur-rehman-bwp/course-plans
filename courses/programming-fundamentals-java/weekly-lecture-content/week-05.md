# Week 5 — Lecture Content: Methods

## 1. Why Methods?
A method is a named, reusable block of code. Decomposing a program into small methods — each
doing one clear thing — makes it easier to read, test, and debug than one long `main`.

## 2. Declaring and Calling a Method
```java
public class Circle {
    public static void main(String[] args) {
        double area = circleArea(5.0);
        System.out.println("Area: " + area);
    }

    static double circleArea(double radius) {
        return Math.PI * radius * radius;
    }
}
```
A method declaration has a **return type** (`double`, or `void` for none), a **name**
(`circleArea`), and a **parameter list** (`double radius`). Here, `circleArea` is `static` because
it is called directly from another `static` method (`main`) without creating an object — the same
reason `main` itself is `static`. (Week 9 introduces non-`static`, instance methods.)

## 3. Pass-by-Value — and What That Means for References
Java is **always** pass-by-value: a method receives a *copy* of what the caller passed. The
subtlety is in *what* gets copied for each kind of type.

**Primitives** — the value itself is copied. Reassigning the parameter inside the method never
affects the caller's variable:
```java
static void tryToDouble(int n) {
    n = n * 2;   // only changes the local copy
}

public static void main(String[] args) {
    int x = 5;
    tryToDouble(x);
    System.out.println(x);   // still 5 — unchanged
}
```

**Object/array references** — the *reference* (the "address") is copied, not the object itself.
Both the caller's variable and the parameter now refer to the **same** object, so changes made
*through* the parameter to the object's contents are visible to the caller:
```java
static void zeroOutFirst(int[] arr) {
    arr[0] = 0;          // mutates the shared array's contents — caller sees this
}

static void reassign(int[] arr) {
    arr = new int[]{9, 9, 9};   // only rebinds the LOCAL copy of the reference — caller unaffected
}

public static void main(String[] args) {
    int[] values = {1, 2, 3};

    zeroOutFirst(values);
    System.out.println(values[0]);    // 0 — the shared array was mutated

    reassign(values);
    System.out.println(values[0]);    // still 0 — reassign only changed its own local reference
}
```
This is the single most important mental model in Java method parameters: **you can change what
an object contains through a reference parameter, but you cannot make the caller's variable point
to a different object.** The same rule applies to any object, not just arrays.

## 4. Method Overloading
```java
static double discount(double price, double percentOff) {
    return price - (price * percentOff / 100.0);
}

static double discount(double price, double percentOff, double maxDiscount) {
    double amountOff = price * percentOff / 100.0;
    if (amountOff > maxDiscount) {
        amountOff = maxDiscount;
    }
    return price - amountOff;
}
```
**Overloading** lets several methods share a name as long as their parameter lists differ (in
number or type). The compiler picks the matching overload based on the arguments at each call
site. Overloading is resolved at compile time, purely from the method signature — it has nothing
to do with return type alone (two methods cannot be overloads if they differ only in return type).

## 5. Scope and Lifetime of Local Variables
```java
static int addOne(int n) {
    int result = n + 1;   // result exists only inside this method call
    return result;
}
// result is gone as soon as addOne returns; a new call creates a fresh one
```
A local variable (including a parameter) exists only for the duration of its method call. Each
call gets its own fresh set of local variables — this is also why recursive calls (Week 12) don't
interfere with each other's local variables.

## 6. In-Class Exercise
Write two methods: `addOneToPrimitive(int n)` that adds `1` to its parameter, and
`addOneToFirstElement(int[] arr)` that adds `1` to `arr[0]`. Call both from `main`, printing the
original variable before and after each call, and explain in a comment why one call changes the
caller's value and the other does not.

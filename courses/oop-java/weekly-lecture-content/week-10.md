# Week 10 — Lecture Content: Generics I — Generic Classes

## 1. The Problem Generics Solve
```java
public class ObjectBox {
    private Object content;
    public void set(Object content) { this.content = content; }
    public Object get() { return content; }
}

ObjectBox box = new ObjectBox();
box.set("hello");
String s = (String) box.get();   // unchecked cast required — and the compiler can't verify it's safe

box.set(42);                      // compiles — nothing stopped a wrong type going in
String oops = (String) box.get(); // compiles, then throws ClassCastException at RUNTIME
```
Before generics, a reusable container had to store `Object` and rely on casts the compiler
cannot check. Mistakes like `oops` above compile cleanly and only fail at runtime, often far from
where the real error was made.

## 2. A Generic Class: `Box<T>`
```java
public class Box<T> {
    private T content;

    public void set(T content) { this.content = content; }
    public T get() { return content; }
}

Box<String> stringBox = new Box<>();
stringBox.set("hello");
String s = stringBox.get();        // no cast needed — the compiler already knows get() returns String

// stringBox.set(42);               // COMPILE ERROR — caught here, not at runtime
```
`T` is a **type parameter** — a placeholder filled in with a real type (`String`, in
`Box<String>`) at the point of use. The compiler now enforces type safety for you: it rejects
`stringBox.set(42)` immediately, and `get()` returns an already-typed `String` with no cast.

## 3. Two Type Parameters: `Pair<T, U>`
```java
public class Pair<T, U> {
    private T first;
    private U second;

    public Pair(T first, U second) {
        this.first = first;
        this.second = second;
    }

    public T getFirst() { return first; }
    public U getSecond() { return second; }
}

Pair<String, Integer> entry = new Pair<>("apples", 12);
System.out.println(entry.getFirst() + ": " + entry.getSecond());   // apples: 12
```
A generic class can declare more than one type parameter, each independent. `Pair<String,
Integer>` and `Pair<Integer, Integer>` are both valid, distinct instantiations of the same one
`Pair` class — no duplicated class needed per type combination.

## 4. Using the Same Generic Class for Different Types
```java
Box<String> names = new Box<>();
names.set("Ada");

Box<Integer> counts = new Box<>();
counts.set(42);
```
One `Box<T>` class definition now serves every type, each instantiation fully type-checked and
completely independent of the others — exactly the genuine type independence that motivated
generics in the first place.

## 5. Bounded Type Parameters (Brief)
```java
public class NumericBox<T extends Number> {   // T must be a Number (or a subclass of Number)
    private T content;
    public void set(T content) { this.content = content; }
    public double doubled() { return content.doubleValue() * 2; }   // Number guarantees doubleValue()
}

NumericBox<Integer> nb = new NumericBox<>();
nb.set(5);
System.out.println(nb.doubled());   // 10.0

// NumericBox<String> bad = new NumericBox<>();   // COMPILE ERROR — String is not a Number
```
`<T extends Number>` restricts `T` to `Number` or one of its subclasses (`Integer`, `Double`,
etc.), which lets the class body call `Number`'s methods (like `doubleValue()`) on a `T` value —
something an unbounded `<T>` would not allow, since a plain `T` is only guaranteed to be an
`Object`.

## 6. Raw Types: A Warning Sign, Not a Feature
```java
Box rawBox = new Box();          // raw type — no type argument supplied
rawBox.set("hello");
rawBox.set(42);                  // compiles with a warning — defeats the entire point of generics
Object result = rawBox.get();    // back to needing a cast, exactly like the pre-generics ObjectBox
```
Using a generic class without its type argument (a **raw type**) compiles, but only for backward
compatibility with pre-generics code — it produces an unchecked-call warning and throws away
every type-safety guarantee generics exist to provide. Always supply the type argument
(`Box<String>`, or `Box<>` with the diamond operator when the compiler can infer it).

## 7. In-Class Exercise
Implement a generic `Pair<T, U>` with a method `Pair<U, T> swapped()` that returns a new pair with
the two values swapped (and swapped type parameters). Instantiate it with at least two different
type combinations and print both the original and swapped pairs.

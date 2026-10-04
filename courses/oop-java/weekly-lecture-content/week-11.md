# Week 11 — Lecture Content: Generics II — Generic Methods, Wildcards, Bridge to Collections

## 1. Generic Methods: A Type Parameter Scoped to One Method
```java
public class Utils {
    public static <T> void printAll(java.util.List<T> items) {   // <T> here, not on the class
        for (T item : items) {
            System.out.println(item);
        }
    }

    public static <T extends Comparable<T>> T max(java.util.List<T> items) {
        T best = items.get(0);
        for (T item : items) {
            if (item.compareTo(best) > 0) {
                best = item;
            }
        }
        return best;
    }
}
```
```java
java.util.List<Integer> nums = java.util.List.of(3, 7, 2, 9, 4);
System.out.println(Utils.max(nums));   // 9 — T is inferred as Integer for this call
```
A generic class (Week 10) ties its type parameter to the whole class. A **generic method** instead
declares its own type parameter (`<T>`, written just before the return type), independent of any
class-level type parameter — useful for a single reusable `static` (or instance) method like
`max` above, whose `T` is inferred fresh at each call site from the arguments passed.

## 2. Wildcards (Brief)
```java
public static double sumAll(java.util.List<? extends Number> numbers) {
    double total = 0;
    for (Number n : numbers) {
        total += n.doubleValue();
    }
    return total;
}

sumAll(java.util.List.of(1, 2, 3));        // List<Integer> — Integer extends Number, so this fits
sumAll(java.util.List.of(1.5, 2.5));       // List<Double> — also fits
```
`<? extends Number>` means "a list of *some* unknown type that is `Number` or a subclass of it" —
it lets one method accept a `List<Integer>`, a `List<Double>`, and so on, without needing a
separate overload for each. `<? super T>` (the mirror image, accepting `T` or any of its
*super*types) appears in the standard library for methods that only ever *write into* a
collection; recognizing both forms when reading library documentation is the goal here, not
writing elaborate wildcard-bounded APIs yourself yet.

## 3. The Collections Framework Is Just More of This
```java
java.util.List<String> names = new java.util.ArrayList<>();   // List<T> — one type parameter
java.util.Map<String, Integer> ages = new java.util.HashMap<>(); // Map<K, V> — two type parameters
```
`List<T>` and `Map<K, V>` (full treatment in Week 13) are generic classes built from exactly the
ideas covered this week and last: type parameters, and in `Map`'s case, two of them just like
`Pair<T, U>`. Having built your own `Box<T>` and `Pair<T, U>`, the Collections Framework's
generic signatures should already read as familiar rather than new syntax to learn.

## 4. In-Class Exercise
Write a generic method `<T> boolean containsDuplicate(List<T> items)` that returns `true` if any
value in the list appears more than once, using each item's `equals()` (Week 2) for comparison.
Test it with a `List<String>` and a `List<Integer>`.

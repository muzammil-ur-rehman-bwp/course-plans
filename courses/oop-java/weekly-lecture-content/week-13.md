# Week 13 — Lecture Content: The Collections Framework

## 1. `List`/`ArrayList`: A Growable, Type-Safe Array
```java
import java.util.ArrayList;
import java.util.List;

List<String> names = new ArrayList<>();
names.add("Ada");
names.add("Grace");
names.add("Alan");

System.out.println(names.get(1));   // Grace
System.out.println(names.size());   // 3
names.remove("Alan");
```
`ArrayList<T>` grows automatically as elements are added (no fixed size to manage, unlike a plain
array from the prerequisite course), and — being generic (Weeks 10–11) — `List<String>` only ever
holds `String`s, fully type-checked at compile time.

## 2. `Map`/`HashMap`: Key-Value Storage
```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> ages = new HashMap<>();
ages.put("Ada", 36);
ages.put("Grace", 85);

System.out.println(ages.get("Ada"));        // 36
System.out.println(ages.containsKey("Bob")); // false
```
`Map<K, V>` associates each key with one value. Internally, a `HashMap` uses a key's
`hashCode()` to find its bucket and `equals()` to confirm the right entry within it — exactly the
Week 2 contract. A custom class used as a `HashMap` key **must** override both correctly, or
lookups can silently fail to find an entry that is logically present:
```java
public class StudentId {
    private String code;
    public StudentId(String code) { this.code = code; }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof StudentId)) return false;
        return code.equals(((StudentId) obj).code);
    }

    @Override
    public int hashCode() {
        return code.hashCode();   // MUST be consistent with equals(), or HashMap lookups break
    }
}
```

## 3. Iterating with the Enhanced `for` Loop
```java
for (String name : names) {
    System.out.println(name);
}

for (Map.Entry<String, Integer> entry : ages.entrySet()) {
    System.out.println(entry.getKey() + " is " + entry.getValue());
}
```
The enhanced `for` loop works over any `Iterable`, including every `List` and (via
`entrySet()`/`keySet()`/`values()`) every `Map`, without manual index bookkeeping.

## 4. Lambda Expressions: A Quick, Practical Introduction
```java
Runnable r = () -> System.out.println("Running!");   // a lambda implementing Runnable.run()
r.run();   // Running!
```
A **lambda expression** is a compact way to implement an interface that has exactly one abstract
method (a *functional interface*, like `Runnable` or `Comparator`, below) — `() -> body` instead
of writing out a full anonymous class. This course uses lambdas for exactly one purpose: sorting.

## 5. Sorting a `List<CustomObject>` with `Comparator`
```java
import java.util.Comparator;

public class Student {
    private String name;
    private double gpa;

    public Student(String name, double gpa) { this.name = name; this.gpa = gpa; }
    public String getName() { return name; }
    public double getGpa() { return gpa; }

    @Override
    public String toString() { return name + " (" + gpa + ")"; }
}
```
```java
List<Student> students = new ArrayList<>(List.of(
    new Student("Alan", 3.2),
    new Student("Ada", 3.9),
    new Student("Grace", 3.9)
));

students.sort((a, b) -> Double.compare(b.getGpa(), a.getGpa()));   // highest GPA first
System.out.println(students);

students.sort(Comparator.comparing(Student::getGpa).reversed()
                         .thenComparing(Student::getName));        // GPA desc, name asc tie-break
System.out.println(students);
```
`List.sort` takes a `Comparator<T>` — a functional interface with one method, `compare(a, b)` —
and a lambda is the natural way to supply it. `Comparator.comparing(...)` builds one from a
"sort key" method reference, and `.thenComparing(...)` chains a tie-breaker, without you writing
the comparison logic by hand.

## 6. In-Class Exercise
Given a `List<Student>` (as above), write and run three different sorts: by name ascending, by
GPA descending, and by GPA descending with name ascending as a tie-break — each using a lambda or
`Comparator.comparing` chain, printing the list after each sort.

# Week 9 — Lecture Content: Midterm + Classes and Objects

## 1. Midterm Exam
Covers Weeks 1–8 (Java basics, operators, control flow, loops, methods, arrays, strings,
references/`null`). See the Week 8 review materials for practice problems.

## 2. Why Classes?
Up to now, related data has been tracked in separate variables or parallel arrays (e.g., a
`names` array and a matching `scores` array). A **class** lets you define a new type that groups
related data (**fields**) and behavior (**methods**) together into one unit — a first step toward
organizing programs around the things they model, rather than just the operations performed on
loose data.

## 3. Defining a Simple Class
```java
public class Student {
    // fields — the data every Student object carries
    String name;
    double gpa;

    // constructor — runs when a new Student is created with "new"
    public Student(String name, double gpa) {
        this.name = name;   // "this.name" is the field; "name" (right-hand side) is the parameter
        this.gpa = gpa;
    }

    // a method — behavior that operates on this object's own fields
    public void printSummary() {
        System.out.println(name + " — GPA: " + gpa);
    }
}
```
- **Fields** (`name`, `gpa`) are variables that belong to each object individually.
- The **constructor** has the same name as the class and no return type; it runs automatically
  when `new Student(...)` is called, and is responsible for giving the new object's fields their
  initial values.
- `this` refers to "the object this method/constructor is currently running on" — it is needed
  here to distinguish the field `name` from the parameter also named `name`.

## 4. Creating and Using Objects
```java
public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Amara", 3.8);
        Student s2 = new Student("Deng", 3.2);

        s1.printSummary();   // Amara — GPA: 3.8
        s2.printSummary();   // Deng — GPA: 3.2

        System.out.println(s1.name);   // direct field access: "Amara"
    }
}
```
`new Student("Amara", 3.8)` allocates a new `Student` object on the heap, runs its constructor
with the given arguments, and returns a reference to it, stored in `s1`. Each object created this
way has its **own** copy of `name` and `gpa` — `s1` and `s2` are independent `Student` objects.

## 5. Methods Operate on "This" Object's Own Fields
Calling `s1.printSummary()` runs the `printSummary` method with `this` bound to `s1` — so inside
that call, `name` and `gpa` refer to `s1`'s fields specifically. Calling `s2.printSummary()` runs
the exact same method code, but with `this` bound to `s2` instead. One method definition, used by
every object of the class, each time operating on that particular object's own data.

## 6. Scope Note: What's *Not* Covered Yet
This week (and Week 13, which revisits classes) covers only enough of Java's class mechanism to
define and use simple objects. **Inheritance, polymorphism, interfaces, and `abstract` classes are
intentionally deferred** to the follow-on Object-Oriented Programming course — trying to cover
full OOP here would crowd out the arrays/collections/file-I/O/recursion foundation this course is
building.

## 7. In-Class Exercise
Define a class `Point` with `int x` and `int y` fields, a constructor that sets both, and a method
`printLocation()` that prints `"(x, y)"`. Create two different `Point` objects in `main` and print
both.

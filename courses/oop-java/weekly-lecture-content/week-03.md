# Week 3 — Lecture Content: Composition ("Has-A" Relationships)

## 1. Objects as Fields of Another Class
```java
public class Engine {
    private int horsepower;

    public Engine(int horsepower) {
        this.horsepower = horsepower;
    }

    public int getHorsepower() { return horsepower; }

    public String start() { return "Engine roars to life (" + horsepower + " hp)"; }
}

public class Car {
    private String model;
    private Engine engine;   // Car HAS-A Engine — composition

    public Car(String model, Engine engine) {
        this.model = model;
        this.engine = engine;
    }

    public String start() {
        return model + ": " + engine.start();   // delegating to the composed object
    }
}
```
```java
Car car = new Car("Civic", new Engine(158));
System.out.println(car.start());   // Civic: Engine roars to life (158 hp)
```
`Car` does not duplicate `Engine`'s logic — it holds an `Engine` reference and **delegates** to
it. This is **composition**: one class built, in part, from instances of another. `Car` is not an
`Engine` (that would be inheritance, Week 5); it merely *has* one.

## 2. Constructing Composed Objects
```java
public class Car {
    private Engine engine;

    // Option A: caller supplies an already-constructed Engine (used above)
    public Car(Engine engine) {
        this.engine = engine;
    }

    // Option B: Car constructs its own Engine internally
    public Car(int horsepower) {
        this.engine = new Engine(horsepower);
    }
}
```
Both styles are legitimate composition. Passing the composed object in (Option A) is more
flexible — the caller controls exactly which `Engine` a `Car` gets, which matters more once
interfaces (Week 9) make the composed type swappable. Constructing it internally (Option B) is
simpler when the relationship is fixed and private to the class.

## 3. A Class Composed of Several Objects
```java
import java.util.ArrayList;
import java.util.List;

public class Library {
    private String name;
    private List<Book> books = new ArrayList<>();   // composition: Library HAS-A list of Books

    public Library(String name) {
        this.name = name;
    }

    public void addBook(Book book) {
        books.add(book);
    }

    public int bookCount() {
        return books.size();
    }
}

public class Book {
    private String title;
    private String author;

    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    public String getTitle() { return title; }
}
```
`Library` doesn't need to know how `Book` stores its own data — it only needs `Book`'s public
interface. Each class stays responsible for its own fields; `Library` is responsible for
*managing a collection* of `Book`s, not for `Book`'s internal details. (`List`/`ArrayList` get
full treatment in Week 13 — here they are simply the obvious place to hold "many `Book`s.")

## 4. "Has-A" vs. the "Is-A" Relationship Coming Next
```java
// Has-a (composition) — what we did above:
class Car { private Engine engine; }      // a Car HAS-A Engine

// Is-a (inheritance) — starting Week 5:
class Car extends Vehicle { }             // a Car IS-A Vehicle
```
Composition models "has-a": a `Car` *has* an `Engine`, a `Library` *has* `Book`s. Next week we
pause on static members, and in Week 5 we meet inheritance, which models "is-a": a `Car` *is a
kind of* `Vehicle`. Confusing the two is a common design mistake — a `Car` should never `extends
Engine` just to reuse its `start()` method, because a car is not a kind of engine.

## 5. In-Class Exercise
Design a `Playlist` class composed of a `List<Song>`, where `Song` has `title`, `artist`, and
`durationSeconds` fields. Give `Playlist` an `addSong(Song s)` method and a `totalDuration()`
method that sums every composed `Song`'s duration — without `Playlist` ever reaching into
`Song`'s private fields directly.

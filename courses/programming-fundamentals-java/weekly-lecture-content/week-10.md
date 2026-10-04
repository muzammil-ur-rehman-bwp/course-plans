# Week 10 — Lecture Content: Using Existing Classes/APIs

## 1. Why `ArrayList`?
A plain array has a fixed size, chosen when it's created — adding a 6th element to a 5-element
array requires creating an entirely new, larger array and copying everything over by hand.
`java.util.ArrayList<T>` is a built-in class that manages this resizing automatically.

## 2. Creating and Using an `ArrayList`
```java
import java.util.ArrayList;

ArrayList<String> names = new ArrayList<>();

names.add("Amara");       // add("Deng") appends to the end
names.add("Deng");
names.add("Priya");

System.out.println(names.get(1));    // "Deng" — 0-based indexing, like arrays
names.set(1, "Deng Wol");            // replace the element at index 1
names.remove(0);                     // removes "Amara"; everything shifts down one index

System.out.println(names.size());    // 2 — note: size(), a method call, not a .length field
```
`ArrayList<String>` is a **generic** type — the `<String>` says this particular list holds
`String` elements, and the compiler enforces that only `String`s can be added, catching type
mistakes at compile time rather than at run time.

## 3. Iterating an `ArrayList`
```java
for (String name : names) {
    System.out.println(name);
}

for (int i = 0; i < names.size(); i++) {
    System.out.println(i + ": " + names.get(i));
}
```

## 4. `ArrayList` vs. Array — Key Differences
| | Array | `ArrayList` |
|---|---|---|
| Size | Fixed at creation | Grows/shrinks automatically |
| Element type | Primitives or objects | Objects only (see autoboxing below) |
| Length/size | `.length` (field) | `.size()` (method) |
| Adding/removing | Not supported directly | `.add(...)`, `.remove(...)` |
| Access | `arr[i]` | `.get(i)` / `.set(i, value)` |

Use a plain array when the size is fixed and known, or when working with primitives at a scale
where `ArrayList`'s overhead matters. Use `ArrayList` whenever the collection's size needs to
change while the program runs — which, in practice, is most of the time for a growing collection
of records.

## 5. Wrapper Classes and Autoboxing
Java generics (the `<T>` in `ArrayList<T>`) only work with object types, not primitives directly
— you cannot write `ArrayList<int>`. Every primitive type has a corresponding **wrapper class**:

| Primitive | Wrapper class |
|---|---|
| `int` | `Integer` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

```java
ArrayList<Integer> scores = new ArrayList<>();
scores.add(95);          // autoboxing: int 95 is automatically wrapped into an Integer
int first = scores.get(0); // auto-unboxing: Integer is automatically converted back to int
```
**Autoboxing**/**unboxing** is the compiler automatically converting between a primitive and its
wrapper class as needed — you can usually write code as if `ArrayList<Integer>` held plain `int`s
directly, but it's worth knowing the wrapper objects are there underneath (they matter for `==`
comparisons, similar to the `String` pitfall from Week 7 — use `.equals()` or unbox first when
comparing two `Integer`s for equality).

## 6. In-Class Exercise
Rewrite a fixed-size-array version of "keep adding scores until the user types a sentinel" using
`ArrayList<Integer>` instead of an array, and compute the average using `.size()` and a `for-each`
loop.

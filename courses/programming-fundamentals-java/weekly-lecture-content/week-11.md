# Week 11 — Lecture Content: File I/O and Exceptions

## 1. Checked Exceptions and `try`/`catch`
Many file operations in Java can fail in ways outside the program's control (a missing file, a
permissions problem), so the compiler forces you to acknowledge the possibility by declaring or
handling a **checked exception** — most commonly `IOException` for file operations.
```java
try {
    // code that might throw a checked exception
} catch (IOException e) {
    System.out.println("Something went wrong: " + e.getMessage());
} finally {
    // optional: runs whether or not an exception occurred — often used to close resources
}
```
Unlike an unchecked exception (`NullPointerException`, `ArrayIndexOutOfBoundsException` — these
can occur anywhere and are not required to be declared), a checked exception **must** be either
caught or declared with `throws` on the enclosing method, or the code will not compile.

## 2. Reading a File with `Scanner`
```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

try {
    Scanner fileReader = new Scanner(new File("scores.txt"));
    while (fileReader.hasNextLine()) {
        String line = fileReader.nextLine();
        System.out.println(line);
    }
    fileReader.close();
} catch (FileNotFoundException e) {
    System.out.println("Could not find scores.txt: " + e.getMessage());
}
```
`Scanner` works identically whether reading from `System.in` or from a `File` — the same
`nextInt()`, `next()`, `hasNextLine()`, `nextLine()` methods apply. `FileNotFoundException` is a
subtype of `IOException` specifically for a missing or inaccessible file.

## 3. Reading a File Line-by-Line with `BufferedReader`
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

try (BufferedReader reader = new BufferedReader(new FileReader("scores.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    System.out.println("Error reading file: " + e.getMessage());
}
```
`readLine()` returns `null` when there is nothing left to read — that is the loop's natural
stopping condition. The `try (...)` form shown here is **try-with-resources**: it automatically
closes `reader` when the block ends (normally or via an exception), which is the recommended way
to manage file resources rather than remembering to call `.close()` manually.

## 4. Writing a File with `PrintWriter`
```java
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

try (PrintWriter writer = new PrintWriter(new FileWriter("output.txt"))) {
    writer.println("Amara 92");
    writer.println("Deng 88");
} catch (IOException e) {
    System.out.println("Error writing file: " + e.getMessage());
}
```
`PrintWriter` offers the same familiar `print`/`println` methods as `System.out`, just directed
at a file instead of the console. Wrapping it in `FileWriter` targets a specific file by name;
by default, this **overwrites** the file if it already exists (pass `new FileWriter("output.txt",
true)` to append instead).

## 5. A Round Trip: Save, Then Reload
```java
import java.io.*;
import java.util.Scanner;

public class InventoryFile {
    public static void main(String[] args) throws IOException {
        String[] names = {"Widget", "Gadget", "Gizmo"};
        int[] quantities = {10, 5, 20};

        // Save
        try (PrintWriter writer = new PrintWriter(new FileWriter("inventory.txt"))) {
            for (int i = 0; i < names.length; i++) {
                writer.println(names[i] + " " + quantities[i]);
            }
        }

        // Reload
        try (Scanner fileReader = new Scanner(new File("inventory.txt"))) {
            while (fileReader.hasNext()) {
                String name = fileReader.next();
                int qty = fileReader.nextInt();
                System.out.println(name + " -> " + qty);
            }
        }
    }
}
```
This pattern — write one record per line, then read it back token-by-token or line-by-line — is
exactly how the capstone project will persist its data between runs.

## 6. In-Class Exercise
Write a program that attempts to open a file that does not exist, catches the resulting
exception, and prints a friendly message instead of letting the program crash with a raw stack
trace. Then write the save/reload round trip for a small list of `name score` pairs.

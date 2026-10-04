# Lab Manual 3 — Composition

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Practice designing a class composed of one or more other classes, delegating behavior instead of
duplicating it.

## Setup
Create `Lab03.java` (and any supporting classes) in your working folder.

## Procedure
1. **Task A — Compose a class:** design `class Engine` with a `horsepower` field and a
   `start()` method returning a `String`. Design `class Car` with a `model` field and an `Engine`
   field, where `Car`'s constructor accepts an already-constructed `Engine`.
2. **Task B — Delegation:** give `Car` a `start()` method that delegates to its composed
   `Engine.start()`, combining the result with `model`.
3. **Task C — A collection of composed objects:** design `class Playlist` composed of a
   `List<Song>` (`Song` has `title`, `artist`, `durationSeconds`). Add `addSong(Song s)` and
   `totalDuration()` methods that never reach into `Song`'s fields directly.
4. **Task D — Mini-challenge:** add a `longestSong()` method to `Playlist` that returns the
   `Song` with the greatest `durationSeconds`, using only `Song`'s public getters.

## Expected Output
A project with `Engine`, `Car`, `Song`, and `Playlist` classes, compiling cleanly with `javac`,
demonstrating construction and correct delegation.

## Submission
Submit your `.java` files via the course submission system by the end of the lab session.

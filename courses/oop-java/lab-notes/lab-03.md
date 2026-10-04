# Lab Notes 3 — Composition

**Concept recap:** composition models "has-a" — a class holds another class as a field and
delegates to its public interface, rather than reimplementing its behavior. Each class stays
responsible only for its own fields.

**Common pitfalls:**
- Reaching into a composed object's private fields instead of calling its public methods (not
  possible from outside the class anyway, but a common design-intent mistake when fields aren't
  private yet).
- Confusing composition with inheritance — `Car` should never `extends Engine` just to reuse
  `start()`; a car is not a kind of engine.
- Forgetting to initialize a composed field in every constructor path, leaving it `null` and
  causing a `NullPointerException` the first time it's used.
- Letting `Playlist` compute `Song` totals using a raw loop over fields instead of `Song`'s own
  getter methods — breaks encapsulation even though it compiles.

**Debugging tip:** a `NullPointerException` on a composed field almost always means a constructor
path forgot to initialize it — check every constructor, not just the one you tested.

**Instructor tip:** ask students to explain, out loud, why `Playlist` "has-a" `List<Song>` rather
than "is-a" `List<Song>` — this previews Week 14's composition-vs-inheritance design discussion.

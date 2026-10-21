# OOP Memory Visualizer: User Guide

Shows any object-oriented programming (OOP) concept **at memory and object level**, using Java. Instead of only reading about a concept, you watch what exists in memory, in what order it appears, and which reference points to which object.

The default output is a short, step-by-step slide deck.

---

## What problem does it solve?

OOP ideas such as composition, inheritance and polymorphism are often explained with words and UML boxes. That hides the part that confuses many learners: **what is actually in memory?**

This skill answers questions like:

- What really happens when I call `new`?
- Is a reference the same thing as an object?
- If two variables point to the same object, how many objects exist?
- What happens to a part when its owner is cleared?

---

## The core idea: a reference points to an object

Every deck is built on one idea, drawn the same way every time.

| Term | Meaning |
|---|---|
| **Reference** | A variable (on the stack, or a field inside an object) that holds the *address* of an object. It is not the object. |
| **Object** | Lives on the **heap**. |
| **Arrow** | Drawn from the reference to the object, for example `c ──► Car#1`. |

Reading rules:

- Follow the arrow to find the object.
- No arrow means `null`: the reference points to nothing.
- `d = c` draws a second arrow to the **same** object.
- `c = new Car()` moves the arrow to a **new** object.
- `c = null` removes the arrow.

---

## When to use it

Ask in plain language. You do not need to say "slides" or "memory". Examples:

| You say | You get |
|---|---|
| "composition" | A deck showing who creates the part, who holds the pointer, and what happens when the owner is gone |
| "what really happens when I call `new`" | Step-by-step heap allocation and reference assignment |
| "how does `super()` work" | One object with a parent part and a child part |
| "show me how Strategy is wired" | The pattern's objects and arrows drawn out |
| "explain pass by value in Java" | The reference being copied, not the object |

You can also invoke it directly by name: `/oop-memory-visualizer composition`.

---

## Concepts it covers

| Area | Topics |
|---|---|
| **Memory model** | Stack, heap, class area, references and arrows, constructors, `static`, garbage collection, pass by value, `==` vs `equals`, String pool |
| **Four pillars** | Encapsulation, inheritance, polymorphism, abstraction, interfaces |
| **Class relationships** | Association, aggregation, composition, dependency, realization |
| **Design principles** | SOLID, DRY, KISS, YAGNI, composition over inheritance, program to an interface, Law of Demeter, coupling and cohesion |
| **Design patterns** | Singleton, Factory, Builder, Prototype, Adapter, Decorator, Facade, Proxy, Composite, Strategy, Observer, Command, State, Template Method, Iterator |

If you ask for several concepts, you get one deck per concept (or one deck with a section per concept). Two concepts are never squeezed onto one slide.

---

## How it works

1. **Identify the concept** and load the matching reference notes.
2. **Write a tiny Java example** (25 lines at most). The default cast is `Vehicle`, `Car`, `Engine` and `Driver`. If you give your own classes, it uses those.
3. **Storyboard the memory states.** One slide per state change, each showing the stack, heap and class area, with an arrow for every reference.
4. **Build the deck** of 5 to 9 slides.
5. **Reply briefly** with the link, a one-sentence takeaway, and any simplification made.

### Typical slide sequence

| # | Slide | Purpose |
|---|---|---|
| 1 | Title | The concept and the question it answers |
| 2 | The code | The Java example, key line highlighted |
| 3 | Reference primer | Optional, for audiences new to references |
| 4 | Steps (2 to 5 slides) | One memory change per slide |
| 5 | Compare | Optional, the wrong or alternative wiring next to the right one |
| 6 | Recap | Three short takeaways, each tied to a reference or arrow fact |

---

## Worked example: composition

**Code**

```java
class Engine { }
class Car {
    private Engine engine;
    Car() { engine = new Engine(); }
}
Car c = new Car();
c = null;
```

**Memory states shown, one per slide**

| Step | Stack | Heap |
|---|---|---|
| 1. `new Car()` allocates | `c` not set yet | `Car#1` with `engine = null` |
| 2. Constructor runs | `c` not set yet | `Engine#1` created, `Car#1.engine ──► Engine#1` |
| 3. Assignment | `c ──► Car#1` | Same two objects |
| 4. `c = null` | `c` is `null`, old arrow fades | `Car#1` and `Engine#1` unreachable, eligible for GC |

**Takeaway:** the owner creates the part, only one arrow points to it, and when the owner goes the part goes with it.

**Why this differs from aggregation:** in aggregation the `Engine` is created outside and passed in, so two `Car` objects can point to the same `Engine#1`. Clearing one `Car` leaves the engine alive.

---

## Relationship cheat sheet

Each relationship answers one memory question: **who creates the part, who holds the pointer, what happens when the owner is gone?**

| Relationship | Pointer story | Created by | Shared? | When owner is cleared |
|---|---|---|---|---|
| Association (uses-a) | Part is only a method parameter, so a pointer exists only during the call | Outside code | Yes, but nothing stored | Nothing to clean up |
| Aggregation (weak has-a) | Owner stores a field pointing to a part created outside | Outside code | Yes | Part stays alive while another pointer exists |
| Composition (strong has-a) | Owner creates the part and stores the only pointer | The owner | No | Part becomes unreachable with the owner |
| Inheritance (is-a) | One object with a parent part and a child part | `new Child()` | Code shared via class area, state per object | Both parts go together |

Strength, loosest to tightest: **association, aggregation, composition, inheritance.**

---

## Visual conventions

The notation is identical in every deck, so you only learn it once.

- **Three memory cards:** Stack (local variables and references), Heap (objects), Class area (class code and static fields, stored once).
- **Colors:**
  - Parent or interface part: purple
  - Child or concrete part: teal
  - Highlighted line or reference arrow: amber
  - Class link (object to its class): thinner grey arrow labelled `class link`
- **Object ids** such as `Car#1` and `Engine#1`, never invented numeric addresses.
- **Every arrow is labelled** with the reference name.
- **Arrow direction:** always from the reference to the object.
- **Shared object:** two arrows ending at one id means one object with two references.
- **Reassignment:** the old arrow is dashed and fading, the new arrow is solid.
- **Unset reference:** shown as `null (not set yet)` with no arrow.
- **Dashed border:** space reserved or part not built yet.

---

## Accuracy rules

- Local variables and references live on the **stack**. Objects always live on the **heap**.
- A reference holds an address, never the object.
- A reference can also be a **field inside a heap object**. That arrow starts at the field box inside the object.
- An arrow means "holds the address of". It does **not** mean copied, owned or contained. Ownership comes from the relationship type.
- **Primitives** (`int x = 5`) hold the value directly and get no arrow.
- Methods are stored once in the class, not per object.
- Java is **pass by value**: the reference is copied, not the object.
- Access modifiers are compile-time checks, not memory protection.

### Known simplification

The picture is a **simplified model** of the JVM. In reality, class metadata lives in Metaspace, each object header holds a class pointer, and static fields are stored with the class mirror object.

---

## Output formats

1. **Slide deck** from the Slides artifact type, when available (default).
2. **Single HTML page** with next and previous buttons, if no Slides type exists.
3. **Inline diagrams**, one per memory state with a sentence between them, if neither is possible.

---

## Tips for better results

- **Name one concept per request** for the cleanest deck.
- **Paste your own classes** and the skill will reuse them instead of the default `Car` and `Engine`.
- **Ask for a comparison** (for example "composition vs aggregation") to get a side-by-side slide.
- **Ask for a reference primer** if arrows and references are new to you.

---

## Limitations

- Examples are **Java only**.
- It is a teaching model, not a JVM profiler. It does not show real addresses, JIT behavior or exact GC timing.
- Each deck stays lean (text-first, no images, 5 to 9 slides). Ask if you want a part expanded.

---

## Quick reference

| I want to… | Say |
|---|---|
| See a concept in memory | "show `<concept>` at object level" |
| Understand references | "what is a reference, with arrows" |
| Compare two ideas | "`<A>` vs `<B>` in memory" |
| Use my own code | Paste the classes and name the concept |
| Call the skill directly | `/oop-memory-visualizer <concept>` |

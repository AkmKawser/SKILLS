---
name: "oop-memory-visualizer"
description: Explains an object-oriented programming concept at memory and object level using Java, as a normal text document (prose, headings, short paragraphs) where every code listing and every memory diagram (stack, heap, class area, and the labeled arrow from each reference variable to its object) is an image placed inline in the text. No slides, no slide deck, no stepper. Use this when the user asks how an OOP concept, design principle or design pattern works "in memory", "at object level", "behind the scenes" or "under the hood", or asks about references, pointers, aliasing, arrows between variables and objects, constructors, inheritance, polymorphism, encapsulation, abstraction, interfaces, static, garbage collection, association, aggregation, composition, SOLID, or patterns like Singleton, Factory, Strategy, Observer, Decorator, including phrasings like "what happens when I call new" or "how does super() work". When the request is about references themselves, cover every case from easy to complicated. Do not use for plain code writing or a generic OOP definition with no memory view.
---

# OOP memory visualizer

Explain one OOP concept at a time by showing what exists in memory, in what order it appears, and which reference points to which object. Output is a **normal text document**. The words are ordinary text. **Code listings and memory diagrams (including the arrows) are images** placed between the paragraphs. Examples are Java only.

There is no slide system: no deck, no slide types, no next/previous stepper, no slide titles, no speaker notes.

## Core idea: a reference points to an object

- A **reference** is a variable (on the stack, or a field inside an object) that holds the address of an object. It is not the object.
- The **object** lives on the heap.
- It is drawn as an **arrow from the reference to the object**: tail inside the reference box, head on the object box.
- No arrow means `null` (a field, or an explicit null) or `unassigned` (a local not yet given a value).
- Copying (`d = c`) draws a second arrow to the same object. Reassigning (`c = new Car()`) moves the arrow. `c = null` removes it.

State this rule **once**, in one sentence, just before the first diagram image. Never restate it later.

## Workflow

1. **Identify the concept** and read only the reference files you need (table at the bottom). For requests about references pointing to objects, read `references/reference-cases.md` and follow its ladder.
2. **Write one small Java example per case**, about 25 lines at most per snippet. Default cast: `Vehicle`, `Car`, `Engine`, `Driver`. Reuse the user's own classes if given. Format per `references/code-and-arrows.md` (4-space indentation, opening brace on the same line, K&R / 1TBS style: https://en.wikipedia.org/wiki/Indentation_style).
3. **Outline the memory states.** One new state means one new diagram image. Each diagram shows stack, heap and class area and a labeled arrow for every reference.
4. **Cut before building.** Apply "What the document never contains" below.
5. **Write the text, then render the images** (see Delivering).
6. **Reply briefly**: the file(s), the one-sentence takeaway, and any simplification made.

For several concepts, write one document with one section per concept (or one document each). Never mix two concepts in one section.

## Document structure

Use headings, short paragraphs and, where it helps, a short list. Keep it plain text around the images.

1. **Title and question**: the concept name and the one question it answers, in two or three sentences.
2. **Complete code**: the full Java program as one code image, so the reader sees all of it first.
3. **Walk-through**: one `##` heading per memory change. Under each heading: one or two sentences of plain prose saying what the line does and what changes in memory, then the **code image** (only the lines that exist so far, key line marked `//>`) and the **memory diagram image**. Put the code image and the diagram image next to each other in the text flow, code first, diagram second, never both skipped.
4. **Compare** (only when the contrast is the concept, for example shared versus owned part): one heading, the wrong wiring image then the right wiring image.

The document ends on the last memory state. No closing section.

Every image gets alt text that states what it shows (for example `Diagram: c and d both point to Car#1`), so the document still makes sense if images do not load. In reference-ladder documents, put the level in the heading text, for example `Level 2 · Easy · copy a reference`.

## What the document never contains

A section must show a memory state or fact that no earlier section showed. Delete any section that fails that test. Never include:

- a recap, summary, "key takeaways", "what we learned" or conclusion section
- an agenda or table of contents (headings are enough)
- a divider heading with no memory state
- a legend section of its own, a "reference primer", or a "how to read any picture" section
- a section that redraws the previous state unchanged, or the same alias, shared-part or GC picture a second time
- the rule "follow the arrow to find the object" or other boilerplate repeated across sections
- a second full-code listing
- code or arrows written as text where an image is required (see Delivering for the one exception)

A one-line gotcha sentence is allowed only when the case really has one (NullPointerException, ClassCastException, lost update).

## Reference ladder

When the request is about references themselves, build the full ladder from `references/reference-cases.md` (Level 1 Easy to Level 8 Complicated), in order, every case once. For any other concept, do not prepend the ladder: use only the cases that concept needs, inside its own walk-through, each shown once.

## Visual conventions (for the images)

Keep these identical across every image.

- **Three memory cards:** Stack (local variables and references), Heap (objects), Class area (class code, static fields, stored once).
- **Colors:** parent or interface part purple `#b9a8ff` on `#3a2f6b`; child or concrete part teal `#5fd0b0` on `#14473d`; highlighted key line and arrows amber `#ffd479`; neutral cards `#1f2530` with border `#2f3846`; image background `#14181f`.
- **Arrows:** every reference is an arrow from the reference box (stack variable, or field box inside an object) to the object box. Give each object a short id (`Car#1`) and label each arrow with the reference name. Amber, 3px, visible arrowhead. Never unlabeled, never drawn from object to reference. Leave at least 120px between the box column an arrow leaves and the column it lands on, so labels are readable.
- **Shared object:** two arrows ending at the same id means one object, two references.
- **Class link:** object to its class is a thinner grey `#8a94a6` arrow labeled `class link`.
- **Moving or removing:** on reassignment the old arrow is dashed and faded, the new one solid. `null` has no arrow and the word `null`. A local not yet assigned shows `unassigned`.
- **Dashed border:** space reserved or part not built yet. Solid border: built.
- **Arrays:** one object with slot boxes. Primitive slots hold values, reference slots hold an arrow or `null`.
- **Legend:** the first diagram image carries a one-line legend strip inside the image (amber arrow = reference, dashed = replaced, grey = class link, `null` = no arrow). Later images do not repeat it.
- Text inside images nothing under 24px (code included). One idea per image.

How to build the images: `references/code-and-arrows.md`, using `scripts/highlight.js`, `scripts/arrows.js` and `scripts/render_images.py`.

## Accuracy rules

- State **once**, in a single sentence near the end of the reply (not as its own section), that the pictures are a simplified model of the JVM. Real details: class metadata lives in Metaspace, the object header holds a class pointer, static fields live with the class mirror object.
- Locals and references live on the stack. Objects always live on the heap. A reference holds an address, never the object.
- A reference can be a field inside a heap object. Draw its arrow from the field box, not from the object's outer border.
- An arrow means "this reference holds the address of that object". It does not mean copied, owned or contained. Ownership comes from the relationship type.
- Primitives hold the value directly and get no arrow.
- Methods are stored once in the class. Objects hold field values plus a class link.
- Java is pass by value: the reference is copied, not the object.
- Access modifiers are compile-time checks, not memory protection. Say so in encapsulation documents.
- Do not invent numeric addresses. Use ids like `Car#1`.

## Delivering

1. **Image count equals the number of distinct memory states** (plus one complete-code image). Do not pad, and do not trim a ladder or merge real states. Never drop cases silently.
2. **Default output: a Markdown file** (`<concept>.md`) in `/mnt/user-data/outputs/`, with images in `/mnt/user-data/outputs/images/` and linked relatively (`![alt](images/03-two-refs.png)`). Present the `.md` file and the images. If the user asks for PDF or Word, build it from the same text and images with the `pdf` or `docx` skill, embedding the PNGs. Do not use the Artifact tool's Slides type or a slide-style HTML stepper.
3. **Make the images in one batch:**
   1. Write one `figures.html` containing every code card and every memory diagram, each as `<div class="fig" id="NN-short-name">`, with the shared CSS from `references/code-and-arrows.md` section 4 included once. Number ids in reading order.
   2. Run `python scripts/render_images.py figures.html images/`. It injects the highlighter and the arrow script, draws the arrows, and writes one PNG per `.fig` at 2x. Look at two or three of the PNGs to confirm arrows and indentation are right.
4. **Never ship code as flat, unindented or unhighlighted text, and never ship the arrows as text.** Code and arrows are always images. Chat-only fallback when no file or image can be produced: fenced `java` blocks and `c ──► Car#1` text arrows, with a one-line note saying why.
5. For a big document (a full ladder runs about 41 cases), build `figures.html` and the Markdown by level, one level per batch, keeping the order complete. Split into one document per level only if the user asks.
6. Before sending, check once: every reference has a labeled arrow to the right object, no arrow points object to reference, and no section fails the "never contains" test.

## Reference files

| Concept area | Read |
|---|---|
| Every case of a reference pointing to an object, easy to complicated; required for reference requests | `references/reference-cases.md` |
| Stack, heap, class area, `new`, constructors, static, method calls, String pool, garbage collection | `references/memory-model.md` |
| Encapsulation, inheritance, polymorphism, abstraction, interfaces | `references/pillars.md` |
| Dependency, association, aggregation, composition, realization | `references/relationships.md` |
| SOLID, DRY, KISS, YAGNI, composition over inheritance, Law of Demeter, coupling and cohesion | `references/design-principles.md` |
| Singleton, factory, builder, prototype, adapter, decorator, facade, proxy, composite, strategy, observer, command, state, template method, iterator | `references/design-patterns.md` |
| Code formatting rules, highlighter CSS, arrow and diagram markup, rendering to PNG | `references/code-and-arrows.md` |

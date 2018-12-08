# Chapter 4 - Pay Off Your Technical Debt

Refactoring code under heavy development by other devs is hard

## Follow the Principle of Proximity

Sometimes an abstraction leaves code in a *worse* state
- "When we refactor such code into a shared representation we give that new method different reasons to change, and when that happens our shared abstraction breaks down in a heavy rain of control flags and Boolean parameters, which is a worse problem than the original duplication."

Example - Entity Framework Core methods
    -`String_EndsWith_MethodCall`
    -`String_StartsWith_MethodCall`

### Use Your Perception

- **The proximity principle** - a Gestalt psychology theory that specifies that objects or shapes that are close to each other appear to form groups

Example - Entity Framework Core methods
- the methods had about 50 lines of code between them, so they were moved next to each other since they are related

### Automate Proximity Recommendations

Example - Clojure code
- hash-map and array-map methods change together but are 300 lines apart in the same file

### Refactor Congested Code with the Splinter Pattern

Splinter pattern - structured way to break up hotspots into manageable pieces that can be divided among several devs to work on

Main reason code becomes a hotspot - it accumulates several responsibilities

Then it has more reasons to change, starting a downward spiral

### Parallel Development is at Conflict with Refactoring

### Split A Hotspot File Along Its Responsibilities

Break hotspot into smaller parts along responsibilities while maintaining original API for a transient period

1) Ensure yoru tests cover the splinter candidate
2) Identify the behaviors inside your hotspot
3) Refactor for proximity
4) Extract a new module for the behavior with the most development activity
5) Delegate to the new module
6) Perform the necessary regression tests to ensure you haven't altered the behavior of the system
7) Select the next behavior to refactor and start over at step 4

### Separate Code With Mixed Content

- May be difficult to do with libraries like React

### Signal Incompleteness With Names

When splintering, if code is shared, it can
1) Be duplicated in the different splinters, or
2) Maintain a shared abstraction in a new module that both splinters depend on

With option 2, you can indicate low cohesion by giving the module a name like `Dumpster.java`, instead of something like `misc` or `utils`

### Know the Consequences of Splinters

- After splintering, you may find that some modules don't need refactorings; they become stable
- After splintering, run a new analysis to let you prioritize future refactorings
- Don't refactor on a separate branch - the key driver behind a splinter refactoring is short lead times

## Build Temporary Tests as a Saftey Net

- Test coverage is usually inadequate
- You'll need to build a safety net based on *end-to-end* tests
- Types of end-to-end tests depend on the API of your hotspot

### Introduce Provisional End-to-end Tests

- Treat code as a black box and just focus on its visibile behavior
    - Ex. Selenium for web
    - Ex. Sikuli for UI
- Results in brittle test suite, but sets us up for next step - refactoring a core part of the system
- Safety net needs to be temporary, and get rid of it once we meet our goal

- Maintainable Tests Don't Depend on Details
    - don't depend on details of UI
    - instead query the DOM, or the identity of a specific component

### Reduce Debt By Deleting Cost Sinks

- Test code not growing with app code is a warning sign
- Tests not growing with complexity of app code is another warning sign
- May indicate that a sole developer tried to introduce unit tests but the organization as a whole didn't embrace the change
    - These sorts of tests do more harm than good since they don't provide much value but still have to be built
    - Delete them as a cost-saving measure

## Turn Hotspot Methods into Brain-Friendly Chunks

Naming programming constructs ties to our working memory, one of the most limiting factors in programming

Optimizing for programmer understanding is one of the most important choices we make

Ex. Chess masters
- Chess masters don't recall individual pieces; they recall patterns aka chunks

Ex. Craft.net
- Reveals accidental complexity - nested loops & complex conditionals
- Extract each piece into a method stating what it does - these are the "chunks"

Data types are chunks too - replace primitives with types that have meaningful names

## The Curse of a Successful System

Code grows into hotspots because we change it a lot, and those changes are driven by user needs, both real and perceived



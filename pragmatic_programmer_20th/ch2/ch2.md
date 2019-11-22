# Chapter 2 - A Pragmatic Approach

Good design is easier to change than bad design

## DRY - Don't Repeat Yourself

DRY is more than code - it's about the duplication of *knowledge*, of *intent*

Not all code duplication is knowledge duplication

Interdeveloper duplication - different developers on a project can inadvertently duplicate sets of functionality. This can go unnoticed for a long time

## Orthogonality

Eliminate effects between unrelated things

### Coding
Keep your code decoupled
  - Write shy code - don't reveal anything unnecessary to other modules; don't rely on other modules' implementations
Avoid global data
  - Be careful with singletons
Avoid similar functions

## Reversibility

There are no final decisions

Forgo following fads

Make failure inexpensive

## Tracer Bullets

Something that gets us from a requirement to some aspect of the final system quickly, visibly, and repeatably

Advantages
- Users get to see something working early
- Developers build a structure to work in
- You have an integration platform
- You have something to demonstrate
- You have a better feel for progress

Tracer bullets don't always hit their target

Tracer code vs prototyping
- True prototypes are thrown away after putting something together and learning
- Tracer code is lean but complete, and forms part of the skeleton of the final system

Prototype to learn

**Prototypes are disposable**

Domain languages

Estimating

Estimate to avoid surprises

Understand what's being asked
Build a model of the system
Break the model into components
Give each parameter a value
Calculate the answers
Keep track of your estimating prowess

Estimating Project Schedules

Painting the missile
Eating the Elephant

Iterate the schedule with the code

What to say when asked for an estimate - **"I'll get back to you."**

You almost always get better results if you slow the process down and spend some time thinking about what being asked for.
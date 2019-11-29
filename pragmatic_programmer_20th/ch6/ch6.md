# Chapter 6 - Concurrency

## Temporal Coupling

Coupling in time - method A must always be called before method B

Analyze workflow to improve concurrency

Concurrency is a software mechanism; parallelism is a hardware concern

## Shared State is Incorrect State

A semaphore is a thing that only one person can own at a time

Make the resource transactional

Random failures are often concurrency issues


## Actors and Processes

An *actor* is an independent virtual processor with its own local (and private) state. Each actor has a mailbox. When a message appears in the mailbox and the actor is idle, it kicks into life and processes the message. When it finishes processing, it processes another message in the mailbox, or if the mailbox is empty, it goes back to sleep.

When processing a message, an actor can create other actors, send messages to other actors that it knows about, and create a new state when the next message is processed

A *process* is typically a more general-purpose virtual processor, often implemented by the operating system to facilitate concurrency. Processes can be constrained (by convention) to behave like actors, and that's the type of process we mean here.

Actors can only be concurrent

Use actors for concurrency without shared state

## Blackboards

Crime scene blackboard
- None of the detectives need to know of the existence of any other detective - they watch the board for new information, and add their findings
- Detectives may have different disciplines & different levels of expertise. They may not work in the same precinct, but they all share a desire to solve the case
- Different detecives may come and go during the course of the process, and may work different shifts
- There are no restrictions on what may be placed on the blackboard
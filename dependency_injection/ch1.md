# Chapter 1

**Definition** - *Dependency Injection* is a set of software design principles and patterns that enables you to develop loosely coupled code.

### Explain It Like I'm 5

"When you go and get things out of the refrigerator for yourself, you can cause problems. You might leave the door open, you might get something Mommy or Daddy doesn’t want you to have. You might even be looking for something we don’t even have or which has expired.

What you should be doing is stating a need, “I need something to drink with lunch,” and then we will make sure you have something when you sit down to eat."

AKA Collaborating classes should rely on infrastructure to provide necessary services

*Service* - an abstraction, a definition for something that provides a service

*Component* - implementation of an abstraction

## Common DI Myths

- DI is only relevant for late binding
- DI is only relevant for unit testing
- DI is a sort of Abstract Factory on Steroids
- DI requires a DI container

*Late binding* - the ability to replace parts of an application without recompiling the code

Many devs think about DI as a service that can be used to locate other services. This is called a **Service Locator**, but it's the exact opposite of DI

**DI is the opposite of a service locator; it's a way to structure code so that you never have to imperatively ask for dependencies. Rather, you require consumers to supply them.**

A *DI container* is an optional library that makes it easier to compose classes when you wire up an application, but it's in no way required. When you compse applications without a DI container, it's called *Pure DI*.

*Liskov Substitution Principle* - we should be able to replace one implemntation of an interface with another without breaking either the client or the implementation

*Null object pattern* - allows you to replace a real implementation with something that does nothing without causing trouble

*Decorator pattern* - allows you to incrementally introduce new features and cross-cutting concerns without having to rewrite or change a lot of existing code

*Composite pattern* - aggregates several implemntations into one

*Adapter pattern* - matches two related, yet separate, interfaces to each other

*Constructor injection* - the act of statically defining the list of required dependencies by specifying them as parameters to the class's constructor

## Benefits of DI

**Late binding** - services can be swapped with other services without recompiling code
- Valuable in standard software, but perhaps less so in enterprise applications where the runtime environment tends to be well defined

**Extensibility** - code can be extended and reused in ways not explicitly planned for
- Always valuable

**Parallel development** - code can be developed in parallel
- Valuable in large, complex applications; not so much in small, simple applications.

**Maintainability** - classes with clearly defined responsibilities are easier to maintain
- Always valuable.

**Testability**	- classes can be unit tested
- Always valuable.

## What to inject and what not to inject?

*Seam* - a place where an application is assembled from its constituent parts, similar to the way a piece of clothing is sewn together at its seams

Volatile dependencies vs Stable dependencies

### Stable Dependencies

- The class or module already exists
- You expect that new versions won’t contain breaking changes
- The types in question contain deterministic algorithms
- You never expect to have to replace, wrap, decorate, or Intercept the class or module with another

### Volatile Dependencies

- The Dependency introduces a requirement to set up and configure a runtime environment for the application
- The Dependency doesn’t yet exist, or is still in development
- The Dependency isn’t installed on all machines in the development organization
- The Dependency contains nondeterministic behavior

Volatile Dependencies are the focal point of DI. It’s for Volatile Dependencies rather than Stable Dependencies that you introduce Seams into your application. Again, this obligates you to compose them using DI.

## DI Scope

*Object composition* - the task of creating instances of dependencies

As developers, we gain control by removing a class’s control over its Dependencies. This is an application of the Single Responsibility Principle. Classes shouldn’t have to deal with the creation of their Dependencies.

DI gives you an opportunity to manage Dependencies in a uniform way. When consumers directly create and set up instances of Dependencies, each may do so in its own way. This can be inconsistent with how other consumers do it. You have no way to centrally manage Dependencies and no easy way to address Cross-Cutting Concerns. With DI, you gain the ability to Intercept each Dependency instance and act on it before it’s passed to the consumer. This provides extensibility in applications.

DI is a subset of IoC (Inversion of Control)

The term Inversion of Control originally meant any sort of programming style where an overall framework or runtime controlled the program flow.

Interception is an application of the Decorator design pattern.

Three dimensions of DI
- Object composition
- Object lifetime management
- Interception

Dependency Injection is a means to an end, not a goal in itself. It’s the best way to enable loose coupling, an important part of maintainable code. The benefits you can reap from loose coupling aren’t always immediately apparent, but they’ll become visible over time, as the complexity of a code base grows.

DI must be pervasive. You can’t easily retrofit loose coupling onto an existing code base.

DI is nothing more than a collection of design principles and patterns. It’s more about a way of thinking and designing code than it is about tools and techniques
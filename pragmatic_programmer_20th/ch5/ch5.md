# Chapter 5 - Bend, or Break

## Decoupling

Decoupled code is easier to change

Examples of coupling
- Train wrecks - chains of method calls
- Globalization - the dangers of static things
- Inheritance - why subclassing is dangerous

### Train wrecks

```
public void applyDiscount(customer, order_id, discount) {
  totals = customer
            .orders
            .find(order_id)
            .getTotals();
  totals = grandTotal = totals.grandTotal - discount;
  totals.discount = discount;
}
```

Tell, don't ask

```
public void applyDiscount(customer, order_id, discount) {
  customer
    .findOrder(order_id)
    .applyDiscount(discount)
}
```

### Law of Demeter

A method defined in a class C should only call:
- Other instance methods in C
- Its parameters
- Methods in objects that it creates, both on the stack and in the heap
- Global variables

Don't chain method calls, unless you're calling things that are unlikely to change (e.g. LINQ in C#)

### The Evils of Globalization

Globally accessible data is an insidious source of coupling between application components

Global data includes singletons, and external resources, such as databases, file systems and APIs

If it's important enough to be global, wrap it in an API

**Keep your code amenable to change**

## Juggling the Real World

### Events

An event represents the availability of information

Finite State Machines
The Observer Pattern
Publish/Subscribe
Reactive Programming & Streams

## Transforming Programming

Programming is about code, but programs are about data

Don't hoard state; pass it around

## Inheritance Tax

Inheritance is coupling. Not only is the child class coupled to the parent, the parent's parent, and so on.

Instead of inheritance, use:
- Interfaces & protocols
- Delegation
- Mixins & traits

Prefer interfaces to express polymorphism

Delegate to Services: Has-A Trumps Is-A

Use Mixins to Share Functionality

## Configuration

Parameterize your app using external configuration

Configuration as a service - hide configuration behind a service API

Advantages:
- Multiple apps can share config information, with authentication and access control limiting what each can see
- Configuration changes can be made globally
- The configuration data can be maintained via a specialized UI
- The configuration data becomes dynamic
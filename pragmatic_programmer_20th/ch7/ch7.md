# Chapter 7 - While You Are Coding

## Listen to your lizard brain

Instincts are a response to patterns packed into our nonconscious brain

Play is important - don't be afraid to experiment & prototype. It's okay to fail

## Programming by Coincidence

As developers, we work in minefields. There are hundreds of traps waiting to catch us each day. We should avoid programming by coincidence in favor if *programming deliberately*.

Implicit assumptions - coincidences can mislead at all levels - from generating requirements through to testing

How to program deliberately
- Always be aware of what you are doing
- Can you explain the code, in detail, to a more junior programmer? If not, perhaps you are relying on coincidences
- Don't code in the dark. If you're not sure why something works, you won't know why it fails
- Proceed from a plan
- Rely only on reliable things. Don't depend on assumptions
- Document your assumptions
- Don't just test your code, but your assumptions as well
- Don't be a slave to history. Don't let existing code dictate future code.

## Algorithm Speed

Big-O notation - a mathematical way of dealing with approxmiations
  - An upper bound on the value of the thing we're measuring

- O(1) - Constant (access element in array, simple statements)
- O(lg *n*) - Logarithmic (binary search). The base of the logarithm doesn't matter, so this is equivalent O(log*n*)
- O(n) - Linear (sequential search)
- O(*n* lg *n*) - Worse than linear, but not much worse (average runtime of quicksort, heapsort)
- O(*n*<sup>2</sup>) - Square law (selection & insertion sorts)
- O(*n*<sup>3</sup>) - Cubic (multiplication of two n x n matrices)
- O(C<sup>n</sup>) - Exponential (traveling salesman problem, set partitioning)

## Refactoring

Software is more like *gardening* - it is more organic than concrete

Refactoring - definition from Martin Fowler - A disciplined technique for restructuring an existing body of code, altering its internal structure without changing its external behavior

1. The activity is disciplined, not a free-for-all
2. External behavior does not change

### When should you refactor?

- Duplication - You've discovered a violation of the DRY principle
- Nonorthogonal design - You've discovered something that could be made more orthogonal
- Outdated knowledge - Things change, requirements drift, and your knowledge of the problem increases. The code needs to keep up.
- Usage - As the system gets used by real people under real circumstances, you realize some features are now more important than previously thought, and "must have" feature perhaps weren't.
- Performance - You need to move functionality from one area of the system to another to improve performance
- The Tests Pass - Refactoring should be backed up by good tests

Refactor early, refactor often

### How do you refactor?

- Don't try to refactor and add functionality at the same time
- Make sure you have good tests before you begin refactoring. Run the tests as often as possible. That way you will know quickly if your changes have broken anything.
- Take short, deliberate steps: move a field from class to another, split a method, rename a variable. Refactoring often involves making many localized changes that result in a larger-scale change. If you keep your steps small, and test after each step, you will avoid prolonged debugging.

## Test to code

Testing is not about finding bugs

**A test is the first user of your code**

A function or method that is tightly coupled to other code is hard to test, because you have to set up all that environment before you can even run your method. So making your stuff testable also reduces coupling.

### Test Driven Development

Basic cycle of TDD:
- Decide on a small piece of functionality you want to add
- Write a test that will pass once that functionality is implemented
- Run all tests. Verify that the only failure is the one you just wrote
- Write the smallest amount of code needed to get the test to pass, and verify that the tests now run cleanly
- Refactor your code: see if there is a way to improve on what you just wrote. Make sure the tests still pass when you're done

Top-down or bottom-up? Build end-to-end

## Property-Based Testing

Use property-based tests to validate your assumptions

## Stay Safe Out There

### Security Basic Principles

1) Minimize Attack Surface Area
  - Code complexity leads to attack vectors
  - Input data is an attack vector
  - Unauthenticated services are an attack vector
  - Authenticated services are an attack vector
  - Output data is an attack vector
  - Debugging info is an attack vector
2) Principle of Least Privilege
3) Secure Defaults
4) Encrypt Sensitive Data
  - Don't check in secrets, API keys, SSH keys, encryption passwords or other credentials alongside your source code in version control
5) Maintain Security Updates
  - Apply security patches quickly

Never roll your own crypto

## Naming Things

Your brain treats written words as something to be respected

Name well; rename when needed
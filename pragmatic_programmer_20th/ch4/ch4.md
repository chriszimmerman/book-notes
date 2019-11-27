# Chapter 4 - Pragmatic Paranoia

### You can't write perfect software

## Design By Contract

Bertrand Meyer

Preconditions - what must be true in order for a routine to be called

Postconditions - what must be true after the routine is called

Invariants - ensures a condition is always true from the perspective of a caller


## Dead Programs Tell No Lies

### Crash early

## Assertive Programming

### Use assertions to prevent the impossible

## How to balance resources

Finish what you start - clean up resources

Act locally - nest allocations
- deallocate resources in the opposite order that you allocate them

Take small steps - always
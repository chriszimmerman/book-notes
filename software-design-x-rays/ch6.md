# Spot Your System's Tipping Point

## Is Software Too Hard?

Brain has lots of bottlenecks and biases
- imperfect memory
- restricted attention span
- limited multitasking abilities

This should make coding hard, but we're great at workarounds:
- abstraction
- cohesion
- good naming
- also on a system level
    - group functions in modules
    - aggregate modules into subsystems
    - subsystems compose a system

A system under development is a moving target

As a project grows beyond 12 or 15 developers, coordination, motivation, and communication issues tend to cause a significant cost overhead

### Societies Within a Software System

Ex. Linux repo has 16,241 contributors

### Raise The Abstraction Level

Social analyses on a subsystem level benefit a larger organization better

## Divide and Conquer with Architectural Hotspots

1) Identify architectural boundaries
2) Run hotspot analysis on an architectural level
3) Analyze the files in each architectural hotspot

### A Language for Specifying Architectural Boundaries

![hotspots](ch6_hotspots.png "hotspots")

### Summarize Change Frequencies By Component

## Analyze Subsystems

Exclude all contributions in search except for ones that touch the module of interest

### Prioritize Function Hotspots and Code Clones

Ex. `intel_crtc_page_flip` has 238 lines of code, 82 commits, and a cyclomatic complexity of 52

### Look For Quick Wins

Ex. `intel_finish_page_fli_cs` and `intel_finish_page_flip_mmio` are victims of code duplication. There is very little difference in code in the two functions

### Ask The Right Questions

Ex. function missing in previous example in only one copy of the code. This may indicate a bug.

### Rinse and Repeat

This approach allows you to divide and conquer throughout the codebase

## Fight the Normalization of Deviance




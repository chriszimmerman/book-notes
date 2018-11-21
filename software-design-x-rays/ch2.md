# Chapter 2 - Identify Code With High Interest Rates

## Measure Interest Rates

A pattern in codebases - a power law distribution 
    - X axis is each file in system
    - Y axis is change frequency
    - Lots of files hardly changed & few files changes very frequently

This helps us prioritze code to tackle
    - files with most changes matter most

### Proxy for interest rate

`git log --format=format: --name-only | egrep -v '^$' | sort | uniq -c | sort -r > all_frequencies.txt`

- This is a proxy, and needs improvement.
    - outputs files that no longer exist
    - includes generated files like .csproj and .jar files
- We need to add a new dimension - complexity 
    - McCabe and Halstead measures are language specific
    - *Lines of code* is language agnostic
        - *cloc* is a tool that counts lines of code in files

## Prioritize Technical Debt With Hotspots

Hotspot - a combination of:
- change frequency of each file - proxy for interest rate
- lines of code - simple measure of complexity

"It’s easy to critique code in retrospect. That’s fine as long as we remember that we don’t know the original context in which the code was developed. Code is often written under strong pressures of time constraints and changing requirements. And often that pressure exerted its force while the original developers tried to build an understanding of both the problem and the solution domain. As we inspect the code, perhaps months or years later, we should be careful to not judge the original programmers, but rather use the information we gather as a way forward."

## Evaluate Hotspots with Complexity Trends

### What is complexity, anyway?

nested conditionals - *indentation-based complexity* is a good language-neutral measure

## Use X-Rays to Get Deep Insights into Code

Determine which methods within a file have the most change

- A null check shouldn't be needed in code whose callers you control
- Excess mocking breaks encapsulation

"The length of a test’s setup method is often inversely related to the readability of the code under test. So start from the unit tests when reviewing code; they indicate the design issues you need to look out for in the application code."

## Escape the Technical Debt Trap

Hotspots help identify the higher priority spots in the code to work on

Reasons why code grows into hotspots
- low cohesion
- they tend to stay where they are and remain problematic for years

[CodeScene](https://codescene.io)

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

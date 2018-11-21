# Chapter 3 - Coupling in Time: A Heuristic for the Concept of Surprise

*"The backbone of surprise is fusing speed with secrecy."* - Carl von Clausewitz

*Change coupling*

## Uncover Expensive Change Patterns

Good design - supports change

### What is change coupling?

- two or more files change together over time
- mined from our code's history and evolution

Criteria
    - files are changed within the same commit

To avoid false positives
    - files must change together in at least 20 commits
    - files are changed together in at least 50 percent of the total commits done to either file

Temporal coupling
    - dependencies in call order between different functions
    - ex. you must call A() before B() or else bad things happen

## Detect Cochanging Files

Favorite heuristic - surprise

Surprise
- One of the most expensive things you can put into a software architecture
- Change coupling itself is neither good nor bad; it all depends on context

### Minimize Your Investigative Efforts

`code-maat` is an open source tool for calculating change coupling

### There is no such thing as just test code

Some of the worst hotspots are in automated tests

People strive to keep production code clean, but don't give the same effort to test code

## The Dirty Secret of Copy-Paste

Copy paste detectors are underused in our industry

- For Java & Python - [Clone Digger](https://pypi.org/project/clonedigger/)
- For .NET - [Simian](https://www.harukizaemon.com/simian/index.html)

For implementing one yourself, check out Rabin-Karp algorithm

## The Power of Language-Neutral Analyses

- ex. front-end code changes with server-side logic and DB scripts
    - helps uncover how different pieces of code fit together

### Learn more about change coupling

- Helps determine if our design fits the way we work with the code
- Helps identify missing abstractions in unit tests & possible design issues in corresponding application code

- We can drill deeper from files to change coupling between methods
- Or we can move up and look between components, subsystems, & microservices

## Exercises

[Roslyn](https://codescene.io/projects/1715/jobs/4299/results/code/temporal-coupling/by-commits)

[TensorFlow](https://codescene.io/projects/1714/jobs/4295/results/files/internal-temporal-coupling?file-name=tensorflow/tensorflow/contrib/layers/python/layers/layers.py)

[ASP.NET Core MVC](https://codescene.io/projects/1690/jobs/4245/results/code/temporal-coupling/by-commits)



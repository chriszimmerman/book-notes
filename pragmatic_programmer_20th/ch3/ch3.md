# Chapter 3 - The Basic Tools

## The power of plain text

Keep knowledge in plain text

## Shell games

Use the power of command shells

Configuring your own shell - common changes
- Setting color themes
- Configuring a prompt
- Aliases and shell functions
- Command completion

## Power Editing

Achieve editor fluency

What counts as being "fluent"?
- When editing text, move and make selections by character, word, line, and paragraph
- When editing code, move by various syntactic units (matching delimiters, functions, modules...)
- Reindent code following changes
- Comment and uncomment blocks of code with a single command
- Undo and redo changes
- Split the editor window into multiple panels, and navigate between them
- Navigate to a particular line number
- Sort selected lines
- Search for both strings and regular expressions, and repeat previous searches
- Temporarily create multiple cursors based on a selection or on a pattern match, and edit the text at each in parallel
- Display compilation errors in the current project
- Run the current project's tests

Can you do all this without using a mouse or trackpad?

How to do this
- First, look at yourself while editing. Each time you find yourself doing something repetitive, say to your self, "There must be a better way" then find it
- Once you find a new useful feature, you need to get it installed in your muscle memory, so you can use it without thinking. The only way to do this is through repetition.

## Version Control

One of the most important features for anything - the `undo` key

A Version Control System (VCS) is like a giant `undo` key

Always use version control

## Debugging

Fix the problem, not the blame

**Don't panic** - debugging is just problem solving. Attack it as such.

The best way to start fixing a bug is to first reproduce it.

Write a failing test before fixing the code

Read the damn error message

### Use the binary chop

Use a divide and conquer approach to find the problem
- If a stacktrace is massive, start halfway through and see if the error is there
- If you find bugs on certain datasets, split the dataset in two
- If a bug was introduced in a set of releases, go back to a release halfway between now and the last known working version

### Using logging & tracing

The debugger shows what's going on *now*

Logs and traces can record what happens at certain points

### Rubber ducking

Explain the problem to another person, or an inanimate object

### Don't assume it - prove it

## Text Manipulation

### Learn a text manipulation language

## Engineering Daybooks

Have a book and record:
- what you do
- what you learn
- sketches of ideas
- anything else related to work
- notes in meetings
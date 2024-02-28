---
title: Shell
draft: 
tags:
  - sapling
---
## Shell vs. Bash
A **shell** is a command-line interface that interprets commands and executes them by accepting keyboard input and passing it to the OS to perform. It's a programming environment (similar to Python or Ruby) that includes variables, conditionals, loops, and functions.

The Bourne Again Shell, or **bash**, is a specific type of UNIX shell and one of the most widely used. Another type of shell is Z Shell (**zsh**). While they may vary in the details, at their core they're all roughly the same: they allow you to run programs, give them input, and inspect their output in a semi-structured way.

## `$PATH` and Environment Variables
- If a command doesn't match shell programming keywords, it checks the `$PATH` environment variable
- `$PATH` lists directories for the shell to search for programs
	- When executing a command like `echo`, the shell searches `$PATH` for the corresponding executable file
- Bypassing `$PATH` is possible by providing the full path to the executable file


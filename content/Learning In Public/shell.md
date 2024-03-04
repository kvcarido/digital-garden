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

## Notes from [Missing Semester](https://missing.csail.mit.edu/2020/course-shell/) 
To oversimplify, every program essentially has two "streams" → input and output. Bash also has built in streams ([[Bash Standard Streams]]).

- By default, bash's input is the keyboard, interacting through the terminal
- Following this, bash's output (like when using `echo`) is also the terminal, where users can read text feedback

Shell gives you a way to manipulate where input and output are "fed" into, using special characters

- `<`: "rewire the input for this program to be the contents of this file"
- `>`: "rewire the output of the preceding program into this file"
- `>>`: appends to existing content
- `|`: "take the output of program to the left and make it the input of the program to the right"

```bash
$ echo hello > hello.txt
# nothing gets printed, instead gets written to file hello.txt
```

```bash
$ cat < hello.txt
# shell opens hello.txt, takes its contents and uses it with cat
```

### Root User
On most UNIX-based systems, `root` is a special user with elevated privileges across the OS. This user is (almost) above all access restrictions, and can create, read, update, and delete any file in the system. 

Although best practice is to not login and operate from the root user, you can elevate privileges within the command line using the `sudo` command. Sudo ("sue-doe") lets you "do" something "as su" (super user, or root).


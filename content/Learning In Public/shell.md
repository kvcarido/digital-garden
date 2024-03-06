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

## [Missing Semester](https://missing.csail.mit.edu/2020/course-shell/): The shell 
To oversimplify, every program has two "streams" → input and output. Bash also has [[Bash Standard Streams|built-in streams]].

- By default, bash's input is the keyboard, interacting through the terminal
- Following this, bash's output (like when using `echo`) is also the terminal, where users can read text feedback

Shell gives you a way to manipulate where input and output are "fed" into, using special characters

- `<`: "rewire the input for this program to be the contents of this file"
- `>`: "rewire the output of the preceding program into this file"
- `>>`: appends to existing content
- `|`: "take the output of program to the left and make it the input of the program to the right"

```bash
$ cat < hello.txt
# shell opens hello.txt, takes its contents and uses it with cat
```

```bash
$ echo hello > hello.txt
# nothing gets printed, instead gets written to file hello.txt
```

### Root User
On most UNIX-based systems, `root` is a special user with elevated privileges across the OS. This user is (almost) above all access restrictions, and can create, read, update, and delete any file in the system. 

Although best practice is not to log in and operate from the root user, you can elevate privileges within the command line using the `sudo` command. Sudo ("sue-doe") lets you "do" something "as su" (super user, or root).

## [Missing Semester](https://missing.csail.mit.edu/2020/shell-tools/): Shell Tools and Scripting

| Argument     | Description                                                                                                         |
| :----------- | ------------------------------------------------------------------------------------------------------------------- |
| `$0`         | Name of the script                                                                                                  |
| `$1` to `$9` | Arguments to the script, `$1` is the first function and so on                                                       |
| `$@`         | All the arguments                                                                                                   |
| `$#`         | Number of arguments                                                                                                 |
| `$?`         | Return code of the previous command                                                                                 |
| `$$`         | Process identification number (PID) for the current script                                                          |
| `!!`         | The entire last command including arguments (common example is to run a script with elevated privileges, `sudo !!`) |
| `$_`         | The last argument from the last command                                                                             |

Commands will return output using `stdout` and errors through `stderr`, as well as a **Return Code** to report errors in a more script-friendly manner. The return code (aka exit status) is the way scripts/commands have to communicate how the execution went – a value of 0 usually indicates everything went OK. Anything other than 0 indicates an error occurred. Exit codes can conditionally execute commands using `&&` and `||` operators.

### Manipulating streams and arguments
This is an example script showcasing a few of these features. It will iterate through the arguments provided and search for the string `foobar` using `grep`. The script [[Bash Standard Streams#Standard Error (`stderr`)|redirects]] `stdout` and `stderr` so it won't print anything to the terminal. 

```bash
#!/bin/bash

echo "Start program at $(date)"
# Date will be substituted
echo "Running program $0 with $# arguments with pid $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # When pattern is not found, grep has exit status 1
    # We redirect STDOUT and STDERR to a null register since we do not care about them
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```

The conditional statement uses the `test` utility to compare if the `grep` command's return code (`#?`) doesn't equal (`-ne`) zero. Explore more evaluation options this utility provides using `man test` ([online docs too](https://www.man7.org/linux/man-pages/man1/test.1.html)).

> [!info]
> When performing comparisons in bash, try to use double brackets `[[` `]]` in favor of simple brackets `[` `]`

### Shell Globbing

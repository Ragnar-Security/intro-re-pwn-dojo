# c-compile

## What this is about

A **source file** is the text of a program, written by a person. A **compiler** turns it into a **binary**, a file the computer can run. The whole job is: edit, compile, run. This challenge is the smallest possible lap of that loop.

## Background

Read the module's **"Start here: C from zero"** page first; the part you need is "Concept 1: compiling a program". In short: you write a `.c` file, `gcc` turns it into a program, and you run that program. `printf("...\n")` prints text and `%d` inside it is a placeholder for a whole number.

**How to start:**

1. `/challenge/c-compile` to read your task.
2. `nano ~/solution.c` to write the program (in nano, Ctrl+O then Enter saves, Ctrl+X exits).
3. `gcc ~/solution.c -o ~/solution && ~/solution` to compile and look at what it prints before you hand it to the grader.


## How these challenges work

Most challenges in this dojo give you a program to examine. The C Warmup challenges work the other way around: **you write a small C program, and the challenge checks it.** C is the language most of the programs you will examine later were written in, so writing a little of it first makes the later reading much easier.

Run the challenge with no arguments and it prints your task. Task details differ between challenge instances, so read your own copy:

```
/challenge/c-compile
```

Then write your program, **compile** it (turn the text you wrote into a runnable program), and hand the compiled program to the challenge:

```
nano ~/solution.c
gcc ~/solution.c -o ~/solution
/challenge/c-compile ~/solution
```

`nano` is a simple text editor in the terminal. `gcc` is the C compiler; `-o ~/solution` names the finished program. The challenge runs your program several times with **randomly chosen** inputs and compares what it prints with what it expects, so a program with the answers typed in will not pass. If a test fails, the challenge shows the input it gave, the output it expected, and what your program printed. Read that comparison: it is the best hint available.

Your program runs without any special privileges and cannot read the flag itself. When all tests pass, the challenge prints the flag.

## The challenge

Your program prints two lines: a fixed line of text, then a multiplication with its answer filled in. Run `/challenge/c-compile` to see the exact lines for your instance. The program reads no input.

Every C program has a `main` function, which is where it starts running. `printf` prints text, and `\n` inside the text is a newline (it ends the line). A `%d` inside the text is a placeholder that `printf` fills in with a whole number you pass after the text. Look up `man 3 printf` if you want the full list.

## Hints (open one at a time, stop as soon as you can continue)

1. Start from the smallest program you remember: a file with `#include <stdio.h>`, a `main` function, and one `printf`. Get just the first line passing before you add the second.
2. If `gcc` prints errors, fix only the first one and compile again. Later errors are often just fallout from the first.
3. For the second line, `printf` can print numbers: put a `%d` where each number belongs and pass the numbers after the text. You can pass a multiplication straight in as an argument.

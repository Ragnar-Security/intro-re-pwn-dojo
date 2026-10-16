# c-types

## What this is about

Every value in a program takes up some number of **bytes** (a byte is the basic unit of memory, enough for one small letter). A **type** such as `int` or `char` says how a value is stored and how many bytes it takes. A **struct** bundles several values together under one name.

On this system (64-bit Linux) some types are bigger than you might guess, and a struct can be bigger than its parts added together, because the compiler leaves unused gaps called **padding** so each value sits at a convenient address. When you read compiled programs later, you will see these sizes everywhere as the numbers added to addresses.

## Background

Read "Concept 2: types and sizes" in the module's **"Start here: C from zero"** page. It shows `sizeof`, how to print it with `%zu`, and a toy struct with padding drawn byte by byte. You do not need to know the sizes already: you write a program that asks the compiler.

**How to start:**

1. `/challenge/c-types` to read the list of types and the struct.
2. Write a program with one `printf` line per item. Test it yourself with `gcc -Wall ~/solution.c -o ~/solution && ~/solution`.
3. Hand it to the grader: `/challenge/c-types ~/solution`.


## How these challenges work

Most challenges in this dojo give you a program to examine. The C Warmup challenges work the other way around: **you write a small C program, and the challenge checks it.** C is the language most of the programs you will examine later were written in, so writing a little of it first makes the later reading much easier.

Run the challenge with no arguments and it prints your task. Task details differ between challenge instances, so read your own copy:

```
/challenge/c-types
```

Then write your program, **compile** it (turn the text you wrote into a runnable program), and hand the compiled program to the challenge:

```
nano ~/solution.c
gcc ~/solution.c -o ~/solution
/challenge/c-types ~/solution
```

`nano` is a simple text editor in the terminal. `gcc` is the C compiler; `-o ~/solution` names the finished program. The challenge runs your program several times with **randomly chosen** inputs and compares what it prints with what it expects, so a program with the answers typed in will not pass. If a test fails, the challenge shows the input it gave, the output it expected, and what your program printed. Read that comparison: it is the best hint available.

Your program runs without any special privileges and cannot read the flag itself. When all tests pass, the challenge prints the flag.

## The challenge

Run `/challenge/c-types`. It lists four types and one struct definition. Your program prints the size in bytes of each one, in the order listed and in the format the task shows. It reads no input.

The operator `sizeof` gives the size of a type or value as a number. Print it with `%zu` in `printf`. Use `sizeof` rather than typing numbers: the sizes belong to the platform, and the program should ask the compiler.

## Hints (open one at a time, stop as soon as you can continue)

1. `sizeof(char)` is a number you can print. The program for four types is the same line repeated with different names.
2. For the struct, copy its definition into your program above `main`, exactly as the task shows it, then use `sizeof(struct S)`.
3. If the struct's size is not what you expected from adding up its parts, that is the padding. You do not need to calculate it: the compiler already did, and `sizeof` reports it.

# c-pointers

## What this is about

A **pointer** is a variable that holds the address of another variable, meaning where it lives in memory. Writing `&x` gives the address of `x`. Writing `*p` follows a pointer `p` to the variable it points at, so you can read or change that variable.

Pointers matter because C normally gives a function **copies** of its arguments. A function that wants to change the caller's variables must be handed their addresses instead. Pointer bugs are also behind many of the memory bugs you will meet later in this dojo.

## Background

Read "Concept 3: pointers" in the module's **"Start here: C from zero"** page. The key facts: `&x` is the address of `x`, `*p` is the variable `p` points at, and writing through `*p` changes the original. A function can only change the caller's variables if it receives their addresses.

**How to start:**

1. `/challenge/c-pointers` to read which function you must write and what it must do.
2. On paper, write three example values in three boxes, then work out by hand what the boxes should hold afterwards. This tells you what your program must print.
3. Write the program, compile with `gcc -Wall ~/solution.c -o ~/solution`, and try it with numbers you chose: `echo "1 2 3" | ~/solution` (adjust the input to match the task's format).


## How these challenges work

Most challenges in this dojo give you a program to examine. The C Warmup challenges work the other way around: **you write a small C program, and the challenge checks it.** C is the language most of the programs you will examine later were written in, so writing a little of it first makes the later reading much easier.

Run the challenge with no arguments and it prints your task. Task details differ between challenge instances, so read your own copy:

```
/challenge/c-pointers
```

Then write your program, **compile** it (turn the text you wrote into a runnable program), and hand the compiled program to the challenge:

```
nano ~/solution.c
gcc ~/solution.c -o ~/solution
/challenge/c-pointers ~/solution
```

`nano` is a simple text editor in the terminal. `gcc` is the C compiler; `-o ~/solution` names the finished program. The challenge runs your program several times with **randomly chosen** inputs and compares what it prints with what it expects, so a program with the answers typed in will not pass. If a test fails, the challenge shows the input it gave, the output it expected, and what your program printed. Read that comparison: it is the best hint available.

Your program runs without any special privileges and cannot read the flag itself. When all tests pass, the challenge prints the flag.

## The challenge

Run `/challenge/c-pointers`. It names a function you must write, which takes three `int *` (pointer to int) arguments, and describes how it must change the three variables it points at. Your `main` reads three integers, calls the function with their addresses, and prints the results in the task's format. Input numbers may be negative.

## Hints (open one at a time, stop as soon as you can continue)

1. In `main`, create three plain `int` variables, read them with `scanf("%d", &a)` (the `&` hands `scanf` the address, the same trick you need for your function), and call your function with `&a`, `&b`, `&c`.
2. Inside the function, `*a` means "the variable `a` points to". Reading `*a` gets the number; assigning to `*a` changes the caller's variable.
3. When you need an old value after overwriting a variable, save it in a temporary first. If several results depend on the old values, compute them all before assigning any.

# c-memory

## What this is about

A running program has two main places to keep data. The **stack** holds local variables and is cleaned up automatically when a function returns. The **heap** is memory you ask for yourself with `malloc`, and it stays yours until you give it back with `free`. Nothing frees it for you.

Heap memory is useful when you do not know how much you need until the program runs. It is also where many bugs live: using memory after freeing it, freeing it twice, or writing past the end of it. You will meet those later. Here, practise doing it correctly.

## Background

Read "Concept 5: the stack and the heap" in the module's **"Start here: C from zero"** page, and the array part of "Concept 3". `malloc(n)` returns the address of `n` bytes on the heap (or `NULL`), and `free` gives them back. An array of `N` ints needs `N * sizeof(int)` bytes. Once you have the pointer, you can use it like an array: `data[0]`, `data[1]`, and so on.

**How to start:**

1. `/challenge/c-memory` to read which two facts you must work out and the output format.
2. Write a program that reads `N`, calls `malloc`, reads the numbers into the array, prints them back, and frees the array. Get that working before you add the calculations.
3. Test with your own input: `printf "3\n5 -2 9\n" | ~/solution` (match the task's input format).


## How these challenges work

Most challenges in this dojo give you a program to examine. The C Warmup challenges work the other way around: **you write a small C program, and the challenge checks it.** C is the language most of the programs you will examine later were written in, so writing a little of it first makes the later reading much easier.

Run the challenge with no arguments and it prints your task. Task details differ between challenge instances, so read your own copy:

```
/challenge/c-memory
```

Then write your program, **compile** it (turn the text you wrote into a runnable program), and hand the compiled program to the challenge:

```
nano ~/solution.c
gcc ~/solution.c -o ~/solution
/challenge/c-memory ~/solution
```

`nano` is a simple text editor in the terminal. `gcc` is the C compiler; `-o ~/solution` names the finished program. The challenge runs your program several times with **randomly chosen** inputs and compares what it prints with what it expects, so a program with the answers typed in will not pass. If a test fails, the challenge shows the input it gave, the output it expected, and what your program printed. Read that comparison: it is the best hint available.

Your program runs without any special privileges and cannot read the flag itself. When all tests pass, the challenge prints the flag.

## The challenge

Run `/challenge/c-memory`. Your program reads a count `N`, then `N` integers. It must store them in an array obtained from `malloc`, work out the two facts the task names, print them in the task's format, and call `free` before exiting. Some of the test inputs are all negative or contain a single number, so check your starting values for anything you track.

## Hints (open one at a time, stop as soon as you can continue)

1. `malloc` takes a size in **bytes**, not a number of elements. Think about how many bytes `N` ints take, and use `sizeof(int)` rather than a typed-in number.
2. After `malloc`, check whether the result is `NULL` (no memory was available) before using it.
3. For a "largest" or "smallest" value, start from the first array element rather than from zero, then compare each other element against your running best.

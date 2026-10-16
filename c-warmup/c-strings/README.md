# c-strings

## What this is about

A **string** in C is an array of characters that ends with a special zero byte written `'\0'`, called the **NUL terminator**. There is no separate length stored anywhere: functions find the end of a string by searching for that zero byte. Forgetting the terminator, or making the array too small to hold it, is one of the most common ways C programs go wrong, and one of the first things you will look for in exploitation.

## Background

Read "Concept 4: strings and the NUL terminator" in the module's **"Start here: C from zero"** page. A string is an array of `char` ended by a zero byte. `strlen` counts the characters before that zero, and `sizeof` of the array counts all of its bytes. `fgets` keeps the newline at the end of the line it reads, so remove it before measuring.

**How to start:**

1. `/challenge/c-strings` to read which two facts about the line you must print, and study the worked example in the task.
2. Write a program that reads the line and prints it back, so you know the reading works.
3. Test with your own line: `echo "some words here" | ~/solution`, and compare with what you work out by hand.


## How these challenges work

Most challenges in this dojo give you a program to examine. The C Warmup challenges work the other way around: **you write a small C program, and the challenge checks it.** C is the language most of the programs you will examine later were written in, so writing a little of it first makes the later reading much easier.

Run the challenge with no arguments and it prints your task. Task details differ between challenge instances, so read your own copy:

```
/challenge/c-strings
```

Then write your program, **compile** it (turn the text you wrote into a runnable program), and hand the compiled program to the challenge:

```
nano ~/solution.c
gcc ~/solution.c -o ~/solution
/challenge/c-strings ~/solution
```

`nano` is a simple text editor in the terminal. `gcc` is the C compiler; `-o ~/solution` names the finished program. The challenge runs your program several times with **randomly chosen** inputs and compares what it prints with what it expects, so a program with the answers typed in will not pass. If a test fails, the challenge shows the input it gave, the output it expected, and what your program printed. Read that comparison: it is the best hint available.

Your program runs without any special privileges and cannot read the flag itself. When all tests pass, the challenge prints the flag.

## The challenge

Run `/challenge/c-strings`. Your program reads one line of text (letters, digits and single spaces) and prints exactly two lines about it. Which two facts are asked for depends on your instance, and the task includes a worked example.

Reading a whole line, spaces included, is done with `fgets`, which also keeps the newline at the end of the line. That newline is not part of the text you are asked about, so remove it before you work on the line.

## Hints (open one at a time, stop as soon as you can continue)

1. `char line[128];` then `fgets(line, sizeof line, stdin);` reads a line. Make the array comfortably bigger than the longest line the task allows, with room for the NUL.
2. `strcspn(line, "\n")` returns the position of the first newline, so assigning `'\0'` at that position cuts the newline off. `strlen` then gives the length without it.
3. Work through the string one character at a time with a loop that stops at `'\0'`. The functions in `<ctype.h>` (`isdigit`, `isupper`, `toupper`, ...) and `strtok` (splits a string at a separator) can save work; check `man 3` for each one you use.
